import time
import os
import sys
import subprocess
import urllib.parse
import pyautogui

try:
    import pyperclip
    HAS_PYPERCLIP = True
except ImportError:
    HAS_PYPERCLIP = False

try:
    import tkinter as tk
    HAS_TKINTER = True
except ImportError:
    HAS_TKINTER = False


def cihaz_tipini_tani():
    sistem = sys.platform.lower()
    if "win" in sistem:
        return "windows"
    env_str = " ".join([
        os.environ.get("PATH", ""),
        os.environ.get("PREFIX", ""),
        os.environ.get("ANDROID_ROOT", ""),
        os.environ.get("HOME", ""),
        os.environ.get("TERMUX_VERSION", ""),
    ]).lower()
    if any(k in env_str for k in ["pydroid", "qpy", "android", "com.hipipal", "termux"]):
        return "android"
    return "android" if "linux" in sistem else "windows"


def panoya_kopyala(metin, cihaz):
    if cihaz == "android":
        for cmd in [
            ["termux-clipboard-set", metin],
            ["cmd", "clipboard", "set", "text", metin],
        ]:
            try:
                res = subprocess.run(
                    cmd,
                    stdout=subprocess.DEVNULL,
                    stderr=subprocess.DEVNULL,
                    check=False,
                )
                if res.returncode == 0:
                    return True
            except Exception:
                pass

    if HAS_PYPERCLIP:
        try:
            pyperclip.copy(metin)
            return True
        except Exception:
            pass

    if cihaz == "windows":
        try:
            subprocess.run(
                ["clip"],
                input=metin.encode("utf-16le"),
                stdout=subprocess.DEVNULL,
                stderr=subprocess.DEVNULL,
                check=True,
            )
            return True
        except Exception:
            pass

    if HAS_TKINTER:
        try:
            r = tk.Tk()
            r.withdraw()
            r.clipboard_clear()
            r.clipboard_append(metin)
            r.update()
            r.destroy()
            return True
        except Exception:
            pass

    return False


def basliq_goster(cihaz):
    os.system("cls" if os.name == "nt" else "clear")
    mod = "WINDOWS 10 / 11" if cihaz == "windows" else "ANDROID"
    print("\033[92m")
    print("=================================================================")
    print(" [EDEX-UI KERNEL] INITIALIZING SYSTEM ENVIRONMENT... v7.9.0")
    print(f" [PLATFORM] ACTIVE TARGET OS: {mod}")
    print(" [NETWORKING] SECURE SOCKET LAYER (SSL) - ENCRYPTED TUNNEL")
    print("=================================================================")
    print(r"""
    ██████╗ ██╗      █████╗ ███████╗███████╗     ██████╗ ███████╗███████╗██╗     ██╗     ██╗███╗   ██╗███████╗
    ██╔══██╗██║     ██╔══██╗╚══███╔╝██╔════╝    ██╔═══██╗██╔════╝██╔════╝██║     ██║     ██║████╗  ██║██╔════╝
    ██████╔╝██║     ███████║  ███╔╝ █████╗      ██║   ██║█████╗  █████╗  ██║     ██║     ██║██╔██╗ ██║█████╗  
    ██╔══██╗██║     ██╔══██║ ███╔╝  ██╔══╝      ██║   ██║██╔══╝  ██╔══╝  ██║     ██║     ██║██║╚██╗██║██╔══╝  
    ██████╔╝███████╗██║  ██║███████╗███████╗    ╚██████╔╝██║     ██║     ███████╗███████╗██║██║ ╚████║███████╗
    ╚═════╝ ╚══════╝╚═╝  ╚═╝╚══════╝╚══════╝     ╚═════╝ ╚═╝     ╚═╝     ╚══════╝╚══════╝╚═╝╚═╝  ╚═══╝╚══════╝
                                           /
                          ███████╗██████╗  █████╗ ███╗   ███╗
                          ██╔════╝██╔══██╗██╔══██╗████╗ ████║
                          ███████╗██████╔╝███████║██╔████╔██║
                          ╚════██║██╔═══╝ ██╔══██║██║╚██╔╝██║
                          ███████║██║     ██║  ██║██║ ╚═╝ ██║
                          ╚══════╝╚═╝     ╚═╝  ╚═╝╚═╝     ╚═╝
    """)
    print("=================================================================")
    print(f" AUTHORIZED OPERATOR: BLAZE OFFLINE | MODE: {mod}")
    print("=================================================================" + "\033[0m")


def hedef_secimi_sorus():
    while True:
        print("\n[1] Nömrəyə göndər")
        print("[2] Qrupa göndər (Qrup adı ilə)")
        sec = input("[?] Seçiminiz (1/2): ").strip()
        if sec == "1":
            nomre = input("[?] Hədəf nömrə (məs: +994501234567): ").strip().replace(" ", "")
            if nomre:
                return "numara", nomre
            print("[!] Nömrə boş ola bilməz!")
        elif sec == "2":
            qrup = input("[?] Qrup adı (məs: OFFLINE CALL): ").strip()
            if qrup:
                return "qrup", qrup
            print("[!] Qrup adı boş ola bilməz!")
        else:
            print("[!] Yalnız 1 və ya 2 yazın.")


def whatsapp_ac(hedef_tipi, hedef_deyer, mesaj, cihaz):
    if cihaz == "android":
        panoya_kopyala(mesaj, "android")
        if hedef_tipi == "numara":
            clean_num = hedef_deyer.replace("+", "")
            uri = f"whatsapp://send?phone={clean_num}&text={urllib.parse.quote(mesaj)}"
            subprocess.run(["am", "start", "-a", "android.intent.action.VIEW", "-d", uri, "com.whatsapp"], check=False)
        else:
            subprocess.run(["am", "start", "-n", "com.whatsapp/.Main"], check=False)
        return

    if hedef_tipi == "numara":
        clean_num = hedef_deyer.replace("+", "")
        os.startfile(f"whatsapp://send?phone={clean_num}")
    else:
        os.startfile("whatsapp://")


def windows_gonder(hedef_tipi, hedef_deyer, base_mesaj, adet, saniye_araligi):
    whatsapp_ac(hedef_tipi, hedef_deyer, base_mesaj, "windows")
    print("\n[+] WhatsApp açılır...")
    time.sleep(3.5)

    if hedef_tipi == "qrup":
        pyautogui.hotkey("ctrl", "f")
        time.sleep(0.4)
        panoya_kopyala(hedef_deyer, "windows")
        pyautogui.hotkey("ctrl", "v")
        time.sleep(1.0)
        pyautogui.press("enter")
        time.sleep(0.8)

    for s in range(3, 0, -1):
        print(f"[*] WhatsApp mesaj qutusuna fokuslanın... {s}", end="\r")
        time.sleep(1)
    print("\n[+] Göndərilir...")

    for i in range(1, adet + 1):
        metin = f"{base_mesaj} [{i}]"
        panoya_kopyala(metin, "windows")
        pyautogui.hotkey("ctrl", "v")
        pyautogui.press("enter")

        if i % 100 == 0 or i == adet or i <= 5:
            print(f"[✓] ({i}/{adet}) Göndərildi")

        if i < adet and saniye_araligi > 0:
            time.sleep(saniye_araligi)

    print("\n[✓] Tamamlandı!")


def android_gonder(hedef_tipi, hedef_deyer, base_mesaj, adet, saniye_araligi):
    whatsapp_ac(hedef_tipi, hedef_deyer, base_mesaj, "android")
    print("\n[+] WhatsApp açıldı. Mesaj panoya kopyalandı, yapışdırıb göndərə bilərsən.")
    for i in range(1, adet + 1):
        metin = f"{base_mesaj} [{i}]"
        panoya_kopyala(metin, "android")
        if i % 50 == 0 or i == adet or i <= 5:
            print(f"[{i}/{adet}] Panoya kopyalandı -> {metin}")
        if i < adet and saniye_araligi > 0:
            time.sleep(saniye_araligi)
    print("\n[✓] Tamamlandı!")


def main():
    cihaz = cihaz_tipini_tani()
    while True:
        basliq_goster(cihaz)
        hedef_tipi, hedef_deyer = hedef_secimi_sorus()
        mesaj = input("[?] Göndəriləcək mesaj: ").strip()
        if not mesaj:
            print("[!] Mesaj boş ola bilməz!")
            time.sleep(1.2)
            continue

        try:
            adet = int(input("[?] Neçə ədəd mesaj göndərilsin? (1 - 50000): "))
            adet = max(1, min(50000, adet))
            saniye_araligi = float(input("[?] Mesajlar arası saniyə (məs: 0.2, 0.5, 1): ").replace(",", "."))
            if saniye_araligi < 0:
                saniye_araligi = 0.0
        except ValueError:
            print("[!] Düzgün rəqəm daxil edin!")
            time.sleep(1.2)
            continue

        if cihaz == "windows":
            windows_gonder(hedef_tipi, hedef_deyer, mesaj, adet, saniye_araligi)
        else:
            android_gonder(hedef_tipi, hedef_deyer, mesaj, adet, saniye_araligi)

        tekrar = input("\n[?] Yenidən başlamaq istəyirsən? (E/H): ").strip().upper()
        if tekrar != "E":
            break


if __name__ == "__main__":
    main()
