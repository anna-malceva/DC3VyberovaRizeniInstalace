Postup instalace
===============================

Následující kapitola popisuje postup nasazení veřejné části modulu **Výběrová řízení** na IIS.

Instalace webové aplikace
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Na webovém serveru **veřejné části** v IIS je třeba vytvořit nový aplikační pool s názvem např. 
**DC3VyberovaRizeniPool**. Pool musí být nastaven jako **No managed code** (Bez spravovaného kódu).

2. V rozšiřujícím nastavení poolu je třeba zapnout vlastnost **Load User Profile** (Načíst profil uživatele) = True.

3. Vytvořit novou webovou aplikaci a nasměrovat ji na adresář s buildem veřejné části modulu Výběrová
řízení. Jako pool zvolit nově vytvořený.

Import certifikátů
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Aby se veřejná část modulu **Výběrová řízení** mohla dotazovat na data do DC3, je nutné na **veřejný server** 
naimportovat příslušné certifikáty.

1. Spustit konzolu pro import certifikátů přes Start > Run > mmc

2. Přes menu Soubor -> Přidat nebo odebrat modul snap-in vybrat Certifikáty a zvolit certifikáty na
tomto počítači.

3. Spustit import klientského certifikátu pro ověřování. Spuštění proveďte pomocí .cmd souboru 
**InstallCertVyberovaRizeni.cmd** (spustit jako správce).

4. Import provede nahrání certifikátu **DC3 CA** a vlastního klientského certifikátu **DC3VyberovaRizeni**.
Certifikát DC3 CA musí být přítomen ve složce **Trusted root authorities** (Důvěryhodné kořenové
certifikační autority) a vlastní klientský certifikát **DC3VyberovaRizeni** by se měl objevit ve složce
**Personal** (Osobní). Správnost nahrání klientského certifikátu DC3VyberovaRizeni lze ověřit tak, že při
jeho otevření by měl být **validní**. Pokud by se certifikát z nějakého důvodu nenaimportoval, je třeba ho
naimportovat ručně (heslo uvedené v .cmd souboru).

5. Nyní je nutné pro naimportovaný certifikát nastavit oprávnění na Private key. Pomocí **pravého tlačítka
-> All tasks -> Manage private keys...** přidat účet **IIS APPPOOL\\<jmeno poolu>**. Např. IIS APPPOOL\\DC3VyberovaRizeniPool. 
Úroveň oprávnění nastavit na **Full control**.


Nastavení oprávnění pro DB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
