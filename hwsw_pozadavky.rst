Minimální HW a SW požadavky
===============================

Veřejný webový server
----------------------------

Veřejná část modulu **Výběrová řízení** je webová aplikace v prostředí **ASP.NET**. Pro její instalaci a
následný běh je nutné splnit některé minimální HW a SW požadavky.

- Windows Server 2008 R2 (a vyšší)
- Nastavení jako Web server (tzn. nainstalované IIS)
- SQL Server 2014 Express SP2 LocalDB
- .NET Framework 4.7
- .NET Core 2.1
- ASP.NET Core Runtime package store x64

Potřebná instalační .msi lze stáhnout buď přímo z webu nebo jsou obsažena v dodaných instalačních
podkladech.

Aplikační server pro DC3
----------------------------

Veřejná část modulu Výběrová řízení komunikuje s informačním systémem DC3 po zabezpečeném SSL
kanále. Z tohoto důvodu je nutné, aby interní instalace DC3 byla nakonfigurována pro běh pod HTTPS.
Dále je nutné aby z veřejného webového server byl nastaven prostup na interní server skrze standardní
SSL port 443 nebo jiný vyhrazený.

.. warning:: Provoz po nezabezpečené lince není podporován.

Pokud je interní aplikace DC3 již nastavena a běží pod HTTPS je nutné na webovém serveru IIS povolit,
aby přijímala i SSL klientské certifikáty. Veřejná část modulu Výběrová řízení provádí ověřování proti
interní DC3 prostřednictvím zaslání klientského certifikátu. Z tohoto důvodu je třeba na interní DC3
nastavit příjímání klientských certifikátů.

**Postup:**
Nastavení příjímání klientských certifikátů se provede úpravou souboru **C:\\Windows\\System32\\inetsrv\\config\\applicationHost.config**. 

.. note:: Před úpravou souboru doporučujeme udělat jeho zálohu.

1. Vyhledat iisClientCertificateMappingAuthentication a nastavit **enabled** na **true**

.. code-block:: xml

   <iisClientCertificateMappingAuthentication enabled="true"> </iisClientCertificateMappingAuthentication>

2. Vyhledat sekci **<jméno site>/<jméno virtuálního ardesáře>** a pod ní přidat 2 nové sekce. Začátek cesty **path="Default Web Site/DC3** je třeba upravit shodně podle první sekce.

.. code-block:: xml

    <location path="Default Web Site/DC3/login/certificateauth">
      <system.webServer>
        <security>
          <access sslFlags="Ssl, SslNegotiateCert, SslRequireCert" />
          <authentication>
            <anonymousAuthentication enabled="true" />
            <windowsAuthentication enabled="true" />
          </authentication>
        </security>
      </system.webServer>
    </location>
    <location path="Default Web Site/DC3/vyberova-rizeni-api">
      <system.webServer>
        <security>
          <access sslFlags="Ssl, SslNegotiateCert, SslRequireCert" />
          <authentication>
          <anonymousAuthentication enabled="true" />
          <windowsAuthentication enabled="false" />
          </authentication>
        </security>
      </system.webServer>
    </location>
    
