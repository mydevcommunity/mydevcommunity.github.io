---
layout: post
title: Memahami Dependency Injection
date: 2013-03-22
author: ikhwanhayat
permalink: /memahami-dependency-injection.html
---

__Dependency Injection__ adalah suatu teknik rekabentuk perisian untuk menjadikannya lebih _modular_ dan _flexible_. Ia kadangkala juga disebut sebagai __Dependency Inversion__ atau __Inversion of Control (IoC)__. Maksud sebenar setiap satu frasa sedikit berbeza, namun untuk permulaan bolehlah dianggap semuanya membawa maksud yang hampir serupa.

<!--more-->

## Apa Itu Dependency

Untuk memahami apa itu _Dependency Injection_ (DI), kita perlu terlebih dahulu mengetahui apa yang dimaksudkan dengan _dependency_.

Dalam suatu sistem yang besar, perkara pertama yang perlu dilakukan ialah pecahkan ia kepada bahagian-bahagian yang lebih kecil supaya mudah untuk kita selesaikan dan yang lebih penting mudah untuk kita senggara (_mantainable_).

Katakan kita ingin membina suatu sistem _online_ yang akan memaparkan cuaca bagi suatu tempat. Keperluan yang telah ditetapkan ialah sistem mesti mampu memberitahu pengguna cuaca di bandar mereka melalui alamat IP yang dikesan.

Jadi kita akan buatkan suatu _class_ `WeatherService` untuk tujuan ini. Katakan kita sudah mempunyai pangkalan data yang akan memberikan kita nama bandar berdasarkan IP. Ada pangkalan data lain pula yang diberikan pada kita oleh Jabatan Meteorologi yang menyenaraikan ramalan cuaca untuk satu-satu bandar.

Untuk memudahkan pemahaman, kita buatkan dulu ia sebagai applikasi konsol. Kita akan gunakan bahasa C# untuk contoh ini.

```csharp
public class WeatherService
{
    public void ShowForIp(string ip)
    {
        Console.WriteLine("Your IP is " + ip);

        // Hubungi pangkalan data geolocation
        // Dan larikan query untuk dapatkan bandar berdasarkan IP

        var connGeo = new SqlConnection("server1...");
        connGeo.Open();
        var sqlCity = new SqlCommand("select top 1 city from IpCity where ip='" + ip + "'", connGeo);

        var city = (string)sqlCity.ExecuteScalar();
        Console.WriteLine("City: " + city);

        connGeo.Close();

        // Hubungi pula pangakalan data untuk cuaca
        // Dan dapatkan cuaca terkini untuk bandar

        var connWeather = new SqlConnection("server2...");
        connWeather.Open();
        var sqlWeather = new SqlCommand("select top 1 weather from CityWeather where city='" + city + "' and day=" + DateTime.Now.DayOfYear, connWeather);

        var weather = (string)sqlWeather.ExecuteScalar();
        Console.WriteLine("Weather: " + weather);

        connWeather.Close();
    }
}

// Panggil dari applikasi utama

var svc = new WeatherService();
svc.ShowForIp("10.10.10.10");

// Contoh output
Your IP is 10.10.10.10
City: Kuala Lumpur
Weather: Heavy Rain
```

Kita dapati _class_ ini perlu melakukan beberapa perkara iaitu pertama ia menerima input, kemudian ia mencari bandar pengguna, lalu mencari cuaca, dan akhirnya memaparkannya.

Bila dilihat kembali, terlalu banyak kerja yang perlu dilakukan oleh _class_ ini. _Class_ yang baik ialah ianya fokus pada kerjanya sahaja. Bila kita kecilkan skop tugas suatu _class_, kita dapat menjadikan ia lebih "cohesive", tugasnya lebih fokus.

Tugas asasi _class_ ini ialah menerima IP dan memaparkan cuaca bagi IP tersebut. Namun ia tidak dapat berfungsi jika ia tidak dapat mencari bandar untuk IP itu dan seterusnya mencari cuaca untuk bandar tersebut. Apa kata jika kita buatkan _class_ lain untuk melakukan dua tugas tersebut untuknya. _Class_ `WeatherService` pula nanti hanya perlu gunakan _class_ baru yang kita akan buat ini supaya objektifnya dapat dicapai.

```csharp
public class CityFinder
{
    public string FindFromIp(string ip)
    {
        // Hubungi pangkalan data geolocation
        // Dan larikan query untuk dapatkan bandar berdasarkan IP

        var connGeo = new SqlConnection("server1...");
        connGeo.Open();
        var sqlCity = new SqlCommand("select top 1 city from IpCity where ip='" + ip + "'", connGeo);

        var city = (string)sqlCity.ExecuteScalar();

        connGeo.Close();

        return city;
    }
}

public class WeatherFinder
{
    public string FindForCity(string city)
    {
        // Hubungi pangakalan data untuk cuaca
        // Dan dapatkan cuaca terkini untuk bandar

        var connWeather = new SqlConnection("server2...");
        connWeather.Open();
        var sqlWeather = new SqlCommand("select top 1 weather from CityWeather where city='" + city + "' and day=" + DateTime.Now.DayOfYear, connWeather);

        var weather = (string)sqlWeather.ExecuteScalar();

        connWeather.Close();

        return weather;
    }
}


public class WeatherService
{
    public void ShowForIp(string ip)
    {
        Console.WriteLine("Your IP is " + ip);

        var city = new CityFinder().FindFromIp(ip);
        Console.WriteLine("City: " + city);

        var weather = new WeatherFinder().FindForCity(city);
        Console.WriteLine("Weather: " + weather);
    }
}

// Panggil dari applikasi utama

var svc = new WeatherService();
svc.ShowForIp("10.10.10.10");

```

Cuba lihat, _class_ `WeatherService` nampak lebih bersih bukan? Kurang berserabut apabila tugasnya telah dipecahkan kepada _class_ lain. Baik, pada tahap ini anda dapat lihat bagaimana satu _class_ besar dipecahkan kepada _class_ lebih kecil. Aturcara yang baru ini boleh dikatakan lebih _modular_ dari sebelumnya.


Namun, _class_ `WeatherService` ini **bergantung kepada** _class_ `CityFinder` dan `WeatherFinder` untuk berfungsi. Untuk mencapai _modularity_, kita menghadapi satu masalah lain pula iaitu _dependency_ (kebergantungan). Adanya _dependency_ membuatkan perubahan sukar dilakukan, kerana perubahan pada satu tempat akan mempengaruhi tempat lain. Namun jika tiada _dependency_ langsung maka _class_ `WeatherService` ini langsung tidak dapat berfungsi!

## Mengurus Dependency

Kita perlukan cara untuk menguruskan _dependency_ ini. Salah satu caranya ialah menggunakan teknik _Dependency Injection_.

Dalam _class_ `WeatherService` ini, _dependency_ pada CityFinder dan WeatherFinder adalah kuat kerana ia perlu _instantiate_ _class-class_ ini sendiri sebelum menggunakannya. Jika kita ingin mengubahnya pada masa akan datang, kita perlu korek semula _class_ `WeatherService` ini dan lakukan perubahan di dalamnya.

Lebih baik jika tugas untuk _instantiate_ _class-class_ ini dilakukan oleh "orang lain". "Orang lain" ini kemudiannya akan memberikan _instance_ _class-class_ yang diperlukan kepada `WeatherService` untuk digunakan. Mari kita lihat apa yang saya maksudkan.

```csharp

// Anggapkan tiada perubahan pada class CityFinder dan WeatherFinder

public class WeatherService
{
    private CityFinder cityFinder;
    private WeatherFinder weatherFinder;

    public WeatherService(CityFinder cityFinder, WeatherFinder weatherFinder)
    {
        this.cityFinder = cityFinder;
        this.weatherFinder = weatherFinder;
    }

    public void ShowForIp(string ip)
    {
        Console.WriteLine("Your IP is " + ip);

        var city = cityFinder.FindFromIp(ip);
        Console.WriteLine("City: " + city);

        var weather = weatherFinder.FindForCity(city);
        Console.WriteLine("Weather: " + weather);
    }
}

// Panggil dari applikasi utama

var cityFinder = new CityFinder();
var weatherFinder = new WeatherFinder();

var svc = new WeatherService(cityFinder, weatherFinder);
svc.ShowForIp("10.10.10.10");

```

Kita dapat lihat, applikasi utama yang perlu _instantiate_ _class_ `CityFinder` dan `WeatherFinder`, dan kemudiannya _inject_ mereka ke dalam `WeatherService`. Nah, inilah yang dinamakan __Dependency Inversion__ (mengalihkan tugas mengurus _dependency_ ke tempat lain) atau __Dependency Injection__ (memasukkan _dependency_ ke dalam _class_ yang memerlukannya).

Tugas mengawal _dependency_ biasanya diserahkan kepada kod yang berada lebih atas dalam hierarki panggilan kerana ia lebih mudah dikonfigurasikan.


## Lebih Panjang dan Sukar?

Nampaknya ia hanya memanjangkan kod aturcara kita sahaja? Apa bagusnya begini? Untuk mendemonstrasikan kelebihannya, mari kita wujudkan satu situasi perubahan yang biasa berlaku dalam sistem perisian.

Katakan pada suatu hari, Jabatan Meteorologi telah meningkat taraf perkhidmatan IT mereka. Cuaca sekarang boleh diperolehi menggunakan web service yang telah mereka sediakan. Lebih tepat dan terkini. Kita ingin mengubahkan sistem kita supaya dapat menggunakan web service ini dan tidak perlu bersusah-payah mengambil data dari mereka setiap minggu :)

```csharp

//
// Dalam C#, cara yang saya tunjukkan ini menggunakan interface. Dalam bahasa lain mungkin ia tidak diperlukan.
//
public interface IWeatherFinder
{
    string FindForCity(string city);
}

//
// Ubah sedikit untuk class WeatherFinder asal supaya ia masih dapat digunakan
//
public class WeatherFinderFromDb : IWeatherFinder
{
    public string FindForCity(string city)
    {
        // ... kod sama
    }
}

//
// Class yang baru untuk mencari menggunakan web service
//
public class WeatherWebService : IWeatherFinder
{
    public string FindForCity(string city)
    {
        // Hubungi web service
        // Dapatkan cuaca untuk bandar yang diberi
        // Anggaplah kod selengkapnya ada di sini :)

        return weather;
    }
}

public class WeatherService
{
    private CityFinder cityFinder;

    private IWeatherFinder weatherFinder; // gunakan interface, bukan concrete class lagi

    public WeatherService(CityFinder cityFinder, IWeatherFinder weatherFinder)
    {
        this.cityFinder = cityFinder;
        this.weatherFinder = weatherFinder;
    }

    public void ShowForIp(string ip)
    {
        Console.WriteLine("Your IP is " + ip);

        var city = cityFinder.FindFromIp(ip);
        Console.WriteLine("City: " + city);

        var weather = weatherFinder.FindForCity(city);
        Console.WriteLine("Weather: " + weather);
    }
}

// Panggil dari applikasi utama

var cityFinder = new CityFinder();
var weatherFinder = new WeatherWebService(); // gunakan class yang baru

var svc = new WeatherService(cityFinder, weatherFinder);
svc.ShowForIp("10.10.10.10");

```

Kita dapat lihat bahawa hanya perubahan yang sedikit perlu dilakukan pada `WeatherService`. Malah jika anda menggunakan interface dari awal (satu lagi prinsip yang baik untuk digunakan), maka perubahan langsung tidak dilakukan dalam _class_ `WeatherService` tersebut.

Melalui teknik ini juga, pengujian dapat dilakukan dengan lebih mudah. Katakan kita buatkan satu _unit test_ untuk `WeatherService` ini. Kita tidak mahu web service itu dipanggil setiap kali, kerana ia pastilah lambat. Kita anggapkan sahaja web service ini sudah terbukti berfungsi kerana kita telah lakukan satu lagi _unit test_ untuknya ditempat lain.

Maka kita buatkan satu `WeatherFinderStub` yang memulangkan cuaca yang sudah kita tentukan. _Stub_ ini kemudiannya kita gunakan untuk menguji `WeatherService`.

```csharp

public class WeatherFinderStub : IWeatherFinder
{
    public string FindForCity(string city)
    {
        return "Cloudy";
    }

}

// Dalam unit test

void TestWeatherServiceCanReturnWeatherForIp()
{
    var svc = new WeatherService(new CityFinder(), new WeatherFinderStub()); // gunakan stub

    // Lakukan asserts
}

```


## Dependency Injection Container

_Depedency_ mungkin boleh jadi kompleks contohnya jika `WeatherFinder` itu sendiri perlu bergantung pada _value_ atau _object_ lain. Namun _dependency chain_ seperti ini sebenarnya adalah perkara biasa dalam sistem perisian. Agak sukar sebenarnya jika kita perlu susun sendiri semua _dependency_ ini setiap kali ingin menggunakan _class_ kita.

Katakan _class_ `CityFinder` telah kita ubah supaya _constructor_nya menerima suatu `ConnectionPool` yang menguruskan hubungan ke pangkalan data. Jadi `CityFinder` tidak perlu tahu server mana yang perlu dihubungi. `WeatherWebService` pula menerima URL untuk webservice itu supaya kita mudah melakukan konfigurasi. Ini bermakna _dependency_ kepada hubungan pangkalan data atau URL ditelah dilonggarkan dari _class_ yang asal.

Situasi seperti ini dapat dipermudahkan dengan menggunakan _framework-framework_ _DI Container_ atau kadangkala disebut _IoC Container_. _DI Container_ biasanya ada fungsi automatic injection untuk menguruskan _dependency_ secara automatik. Contoh _framework_ seperti ini yang ada di dalam dunia .NET adalah seperti Castle Windsor, Autofac, Ninject, dan sebagainya.

_Container_ ini biasanya berfungsi seperti berikut:

```csharp

// DiContainer adalah sebuah framework khayalan

var container = new DiContainer();

container.Register<IConnectionPool>().Using<ConnectionPool>()
    .WithParamater("connString", "server1...");

container.Register<ICityFinder>().Using<CityFinder>();

container.Register<IWeatherFinder>().Using<WeatherWebService>()
    .WithParameter("url", "http://webservice/...");

container.Register<IWeatherService>().Using<WeatherService>();

// Panggil dari applikasi utama

var svc = container.GetObject<IWeatherService>(); // Dapatkan WeatherService dari container
svc.ShowForIp("10.10.10.10");

```

Container akan mengambil tugas membina `WeatherService` dan menyambungkan semua _dependency_ nya supaya ia dapat hidup dan berfungsi. Kita tidak perlu menguruskannya sendiri.


## Kesimpulan

_Dependency Injection_ tidak akan dapat difahami jika kita tidak memahami apa itu _dependency_ dan bagaimana ia boleh wujud. Setelah memahaminya, _Dependency Injection_ dapat digunakan untuk mengurus _dependency_ dan seterusnya membantu kita mencapai _modularity_ dan _flexibility_ yang diidam-idamkan dalam sistem kita. Gunakan juga _DI Container_ untuk memudahkan tugas kita menghubungkan _dependency_ antara _object_.

_NOTA: Kod dalam artikel ini adalah khusus untuk memahami Dependency Injection. Ia mungkin mengabaikan aspek lain seperti keselamatan dan sebagainya._

Perbincangan mengenai artikel ini ada di [sini](https://plus.google.com/105721265741813048018/posts/Av85xxdHXUx).
