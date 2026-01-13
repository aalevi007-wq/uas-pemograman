# uas-pemograman
# ==================== CLASS DATA ====================
class Mahasiswa:
    """Class untuk merepresentasikan data mahasiswa"""
    
    def __init__(self, nim, nama, jurusan, ipk):
        self.nim = nim
        self.nama = nama
        self.jurusan = jurusan
        self.ipk = ipk
    
    def __str__(self):
        return f"NIM: {self.nim}, Nama: {self.nama}, Jurusan: {self.jurusan}, IPK: {self.ipk}"


# ==================== CLASS PROCESS ====================
class MahasiswaProcess:
    """Class untuk mengelola proses bisnis data mahasiswa"""
    
    def __init__(self):
        self.data_mahasiswa = []
    
    def tambah_mahasiswa(self, mahasiswa):
        """Menambahkan data mahasiswa baru"""
        # Validasi NIM tidak duplikat
        if any(mhs.nim == mahasiswa.nim for mhs in self.data_mahasiswa):
            raise ValueError(f"NIM {mahasiswa.nim} sudah terdaftar!")
        self.data_mahasiswa.append(mahasiswa)
        return True
    
    def hapus_mahasiswa(self, nim):
        """Menghapus data mahasiswa berdasarkan NIM"""
        for i, mhs in enumerate(self.data_mahasiswa):
            if mhs.nim == nim:
                del self.data_mahasiswa[i]
                return True
        raise ValueError(f"NIM {nim} tidak ditemukan!")
    
    def update_mahasiswa(self, nim, nama=None, jurusan=None, ipk=None):
        """Mengupdate data mahasiswa"""
        for mhs in self.data_mahasiswa:
            if mhs.nim == nim:
                if nama:
                    mhs.nama = nama
                if jurusan:
                    mhs.jurusan = jurusan
                if ipk is not None:
                    mhs.ipk = ipk
                return True
        raise ValueError(f"NIM {nim} tidak ditemukan!")
    
    def cari_mahasiswa(self, nim):
        """Mencari mahasiswa berdasarkan NIM"""
        for mhs in self.data_mahasiswa:
            if mhs.nim == nim:
                return mhs
        return None
    
    def get_all_mahasiswa(self):
        """Mendapatkan semua data mahasiswa"""
        return self.data_mahasiswa


# ==================== CLASS VIEW ====================
class MahasiswaView:
    """Class untuk menampilkan data ke layar"""
    
    @staticmethod
    def tampilkan_header():
        print("\n" + "="*70)
        print("          PROGRAM MANAJEMEN DATA MAHASISWA")
        print("="*70)
    
    @staticmethod
    def tampilkan_menu():
        print("\n" + "-"*70)
        print("MENU:")
        print("1. Tambah Data Mahasiswa")
        print("2. Tampilkan Semua Data")
        print("3. Cari Mahasiswa")
        print("4. Update Data Mahasiswa")
        print("5. Hapus Data Mahasiswa")
        print("6. Keluar")
        print("-"*70)
    
    @staticmethod
    def tampilkan_tabel(data_mahasiswa):
        """Menampilkan data dalam bentuk tabel"""
        if not data_mahasiswa:
            print("\n⚠ Data mahasiswa kosong!")
            return
        
        print("\n" + "="*70)
        print(f"{'NIM':<15} {'NAMA':<20} {'JURUSAN':<20} {'IPK':<10}")
        print("="*70)
        for mhs in data_mahasiswa:
            print(f"{mhs.nim:<15} {mhs.nama:<20} {mhs.jurusan:<20} {mhs.ipk:<10.2f}")
        print("="*70)
    
    @staticmethod
    def tampilkan_detail(mahasiswa):
        """Menampilkan detail satu mahasiswa"""
        if mahasiswa:
            print("\n" + "-"*70)
            print("DETAIL MAHASISWA:")
            print(f"NIM     : {mahasiswa.nim}")
            print(f"Nama    : {mahasiswa.nama}")
            print(f"Jurusan : {mahasiswa.jurusan}")
            print(f"IPK     : {mahasiswa.ipk:.2f}")
            print("-"*70)
        else:
            print("\n⚠ Mahasiswa tidak ditemukan!")
    
    @staticmethod
    def tampilkan_pesan(pesan, tipe="info"):
        """Menampilkan pesan ke user"""
        if tipe == "sukses":
            print(f"\n✓ {pesan}")
        elif tipe == "error":
            print(f"\n✗ ERROR: {pesan}")
        else:
            print(f"\nℹ {pesan}")


# ==================== VALIDASI INPUT ====================
class InputValidator:
    """Class untuk validasi input"""
    
    @staticmethod
    def validasi_nim(nim):
        """Validasi NIM harus berupa angka dan tidak kosong"""
        if not nim:
            raise ValueError("NIM tidak boleh kosong!")
        if not nim.isdigit():
            raise ValueError("NIM harus berupa angka!")
        if len(nim) < 5:
            raise ValueError("NIM minimal 5 digit!")
        return nim
    
    @staticmethod
    def validasi_nama(nama):
        """Validasi nama tidak boleh kosong"""
        if not nama or nama.strip() == "":
            raise ValueError("Nama tidak boleh kosong!")
        return nama.strip()
    
    @staticmethod
    def validasi_jurusan(jurusan):
        """Validasi jurusan tidak boleh kosong"""
        if not jurusan or jurusan.strip() == "":
            raise ValueError("Jurusan tidak boleh kosong!")
        return jurusan.strip()
    
    @staticmethod
    def validasi_ipk(ipk_str):
        """Validasi IPK harus berupa angka antara 0-4"""
        try:
            ipk = float(ipk_str)
            if ipk < 0 or ipk > 4:
                raise ValueError("IPK harus antara 0.00 - 4.00!")
            return ipk
        except ValueError:
            raise ValueError("IPK harus berupa angka!")


# ==================== MAIN PROGRAM ====================
def main():
    process = MahasiswaProcess()
    view = MahasiswaView()
    validator = InputValidator()
    
    view.tampilkan_header()
    
    while True:
        view.tampilkan_menu()
        
        try:
            pilihan = input("Pilih menu (1-6): ").strip()
            
            if pilihan == "1":
                # Tambah Data
                print("\n--- TAMBAH DATA MAHASISWA ---")
                try:
                    nim = validator.validasi_nim(input("NIM: "))
                    nama = validator.validasi_nama(input("Nama: "))
                    jurusan = validator.validasi_jurusan(input("Jurusan: "))
                    ipk = validator.validasi_ipk(input("IPK: "))
                    
                    mahasiswa = Mahasiswa(nim, nama, jurusan, ipk)
                    process.tambah_mahasiswa(mahasiswa)
                    view.tampilkan_pesan("Data mahasiswa berhasil ditambahkan!", "sukses")
                    
                except ValueError as e:
                    view.tampilkan_pesan(str(e), "error")
            
            elif pilihan == "2":
                # Tampilkan Semua Data
                view.tampilkan_tabel(process.get_all_mahasiswa())
            
            elif pilihan == "3":
                # Cari Mahasiswa
                print("\n--- CARI MAHASISWA ---")
                nim = input("Masukkan NIM: ").strip()
                mahasiswa = process.cari_mahasiswa(nim)
                view.tampilkan_detail(mahasiswa)
            
            elif pilihan == "4":
                # Update Data
                print("\n--- UPDATE DATA MAHASISWA ---")
                try:
                    nim = input("NIM mahasiswa yang akan diupdate: ").strip()
                    
                    # Cek apakah mahasiswa ada
                    if not process.cari_mahasiswa(nim):
                        raise ValueError(f"NIM {nim} tidak ditemukan!")
                    
                    print("Kosongkan jika tidak ingin mengubah")
                    nama = input("Nama baru: ").strip()
                    jurusan = input("Jurusan baru: ").strip()
                    ipk_str = input("IPK baru: ").strip()
                    
                    ipk = None
                    if ipk_str:
                        ipk = validator.validasi_ipk(ipk_str)
                    
                    process.update_mahasiswa(nim, nama or None, jurusan or None, ipk)
                    view.tampilkan_pesan("Data mahasiswa berhasil diupdate!", "sukses")
                    
                except ValueError as e:
                    view.tampilkan_pesan(str(e), "error")
            
            elif pilihan == "5":
                # Hapus Data
                print("\n--- HAPUS DATA MAHASISWA ---")
                try:
                    nim = input("NIM mahasiswa yang akan dihapus: ").strip()
                    konfirmasi = input(f"Yakin ingin menghapus NIM {nim}? (y/n): ").lower()
                    
                    if konfirmasi == 'y':
                        process.hapus_mahasiswa(nim)
                        view.tampilkan_pesan("Data mahasiswa berhasil dihapus!", "sukses")
                    else:
                        view.tampilkan_pesan("Penghapusan dibatalkan.")
                        
                except ValueError as e:
                    view.tampilkan_pesan(str(e), "error")
            
            elif pilihan == "6":
                # Keluar
                print("\n" + "="*70)
                print("Terima kasih telah menggunakan program ini!")
                print("="*70)
                break
            
            else:
                view.tampilkan_pesan("Pilihan tidak valid! Pilih 1-6.", "error")
        
        except KeyboardInterrupt:
            print("\n\nProgram dihentikan oleh user.")
            break
        except Exception as e:
            view.tampilkan_pesan(f"Terjadi kesalahan: {str(e)}", "error")


if __name__ == "__main__":
    main()
