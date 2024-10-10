import os
import time
import datetime

class newMutation:
    mutation = []
    def __init__(self) -> None:
        self.mutation = [{"description":"Setor Tunai","total":10000000,"isMinus":False,"date":datetime.datetime.now()}
    def addMutation(self,newMutation):
        self.mutation.append(newMutation)
    def getMutation(self):
        return self.mutation
    
pin =  '123456'
countWrongPin = 3
saldo = 10000000
mutasi = newMutation()

def clear():
    if(os.name == "nt"):
        os.system('cls')
    else:
        os.system('clear')

def format_rupiah(amount):
    rupiah = "Rp {:,.2f}".format(amount).replace(",", ".")
    return rupiah

def kembaliMenuUtama():
    clear()
    print(f"pilih 0 untuk selesai dan lainnya untuk kembali ke menu utama")
    option = input(":")
    if(option=="0"):
        input("Silahkan Ambil Kartu ATM")
        print("Terimakasih Sudah Menggunakan Atm Dengan Baik")
        time.sleep(2)
        clear()
        exit()
    else:
        clear()
        menuUtama()

def masukanPin():
    pinInput = input("silahkan masukan pin anda : ")
    global pin,countWrongPin
    if(pin != pinInput):
        countWrongPin-=1
        print(f"pin yang anda masukan salah tinggal {countWrongPin} percobaan lagi")
        if(countWrongPin == 0):
            print("silahkan hubungi CS Untuk Pengembalian Kartu")
        else:
            masukanPin()
    else:
        clear()
        menuUtama()

def transfer():
    global saldo,mutasi
    kodeBank=input("Masukkan Kode Bank : ")
    noRek=input("Masukan No Rekening : ")
    nominal = int(input("Masukan Nominal Transaksi : "))
    clear()
    print(f"Yakin Ingin Transfer Ke No Rekening {kodeBank}{noRek} dengan Jumlah {format_rupiah(nominal)}")
    option = input("ketik 0 untuk cancel dan lainnya untuk lanjutkan")
    if(option=="0"):
        clear()
        menuUtama()
    else:
        clear()
        if(saldo<nominal):
            print("Saldo Tidak Cukup Silahkan Ulang Kembali")
            time.sleep(2)
            transfer()
        else:
            clear()
            saldo -= nominal
            print("Proses Transfer")
            mutasi.addMutation({"description":f"Transfer ke No Rekening {kodeBank}{noRek}","total":nominal,"isMinus":True,"date":datetime.datetime.now()})
            time.sleep(2)
            kembaliMenuUtama()

def login():
    print("  Selamat Datang Di Bank UBSI   ")
    print('silahkan masukan kartu atm anda!')
    input("")
    clear()
    masukanPin()

def penguranganSaldo(jumblahPenarikan):
    global saldo,mutasi
    if(saldo < jumblahPenarikan):
        print("Saldo Tidak Cukup Silahkan Masukan Nominal Lain")
        input("")
        clear()
        tarikTunai()
    saldo-=jumblahPenarikan
    mutasi.addMutation({"description":f"Tarik Tunai","total":jumblahPenarikan,"isMinus":True,"date":datetime.datetime.now()})
    clear()
    print("Tunggu Uang Sampai Selesai Diporoses")
    time.sleep(2)
    clear()
    print("silahkan ambil uang")
    input("")
    kembaliMenuUtama()


def tarikTunai():
    print(f"                   Pilih Tarik Tunai                  ")
    print("")
    print(f"1. Rp. 100.000,00                     Rp. 500.000,00 2")
    print(f"3. Rp. 1.000.000,00                 Rp. 5.000.000,00 4")
    print(f"5. Masukan Nominal                            Batal  0")
    option  = input(":")
    clear()
    match option:
        case "1":
            penguranganSaldo(100000)
        case "2":
            penguranganSaldo(500000)
        case "3":
            penguranganSaldo(1000000)
        case "4":
            penguranganSaldo(5000000)
        case "5":
            clear()
            nominal = int(input("Silahkan Masukan Nominal Penarikan : "))
            penguranganSaldo(nominal)
        case "0":
            time.sleep(2)
            clear()
            menuUtama()
        case _ :
            print("Silahkan masukan opsi dengan benar")
            time.sleep(2)
            clear()
            menuUtama()

def setorTunai():
    global saldo,mutasi
    print("Silahkan Masukan Uang atau 0 untuk kembali")
    option = input()
    if(option == "0"):
        clear()
        menuUtama()
    else:   
        clear()
        print("Uang Sedang Di Proses ")
        saldo += 50000
        mutasi.addMutation({"description":"Setor Tunai","total":50000,"isMinus":False,"date":datetime.datetime.now()})
        time.sleep(2)
        kembaliMenuUtama()
    
def menuUtama():
    print(f"                 Menu Utama                ")
    print("")
    print(f"1. Informasi Saldo               Transfer 2")
    print(f"3. Cek Mutasi                 Tarik Tunai 4")
    print(f"5. Setor Tunai                     Batal  0")
    option  = input(":")
    clear()
    match option:
        case "1":
            cekSaldo()
        case "2":
            transfer()
        case "3":
            cekMutasi()
        case "4":
            tarikTunai()
        case "5":
            setorTunai()
        case "0":
            input("Silahkan Ambil Kartu ATM")
            print("Terimakasih Sudah Menggunakan Atm Dengan Baik")
            time.sleep(2)
            clear()
            exit()
        case _ :
            print("Silahkan masukan opsi dengan benar")
            time.sleep(2)
            clear()
            menuUtama()

def cekMutasi():
    global mutasi
    index = 0
    for data in mutasi.getMutation():
        index +=1    
        print(f"{index}. {data['description']}")
        print(f"tanggal : {data['date']}")
        print(f"total   : {'-' if data['isMinus'] == True else ''}{format_rupiah(data['total'])}")
        print("")
    input("")
    kembaliMenuUtama()

def cekSaldo():
    global saldo
    print(f"saldo anda tersisa {format_rupiah(saldo)}")
    input("")
    kembaliMenuUtama()

clear()
login()
