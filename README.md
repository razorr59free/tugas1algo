#include <iostream>
#include <string>
using namespace std;

struct Buku {
    string judul;
    int tahun;
};

Buku daftarBuku[100];
int jumlahBuku = 0;

int inputBuku() {
    int n;
    cout << "Jumlah buku yang ingin diinput: ";
    cin >> n;
    cin.ignore();
    for (int i = 0; i < n; i++) {
        cout << "Judul buku ke-" << i + 1 << ": ";
        getline(cin, daftarBuku[jumlahBuku].judul);
        cout << "Tahun terbit: ";
        cin >> daftarBuku[jumlahBuku].tahun;
        cin.ignore();
        jumlahBuku++;
    }
    return jumlahBuku;
}

int tampilBuku() {
    if (jumlahBuku == 0) {
        cout << "Belum ada data buku.\n";
        return 0;
    }
    for (int i = 0; i < jumlahBuku; i++) {
        cout << i + 1 << ". " << daftarBuku[i].judul << " (" << daftarBuku[i].tahun << ")\n";
    }
    return 1;
}

int bubbleSort() {
    for (int i = 0; i < jumlahBuku - 1; i++) {
        for (int j = 0; j < jumlahBuku - i - 1; j++) {
            if (daftarBuku[j].tahun > daftarBuku[j + 1].tahun) {
                Buku temp = daftarBuku[j];
                daftarBuku[j] = daftarBuku[j + 1];
                daftarBuku[j + 1] = temp;
            }
        }
    }
    return 1;
}

int quickSort(int left, int right) {
    int i = left;
    int j = right;
    int tengah = (left + right) / 2;
    int nilaiTengah = daftarBuku[tengah].tahun;

    while (i <= j) {
        while (daftarBuku[i].tahun < nilaiTengah) {
            i++;
        }
        while (daftarBuku[j].tahun > nilaiTengah) {
            j--;
        }
        if (i <= j) {
            Buku temp = daftarBuku[i];
            daftarBuku[i] = daftarBuku[j];
            daftarBuku[j] = temp;
            i++;
            j--;
        }
    }
    if (left < j) {
        quickSort(left, j);
    }
    if (i < right) {
        quickSort(i, right);
    }
    return 1;
}

int sequentialSearch() {
    int tahun;
    bool ditemukan = false;
    cout << "Masukkan tahun terbit yang dicari: ";
    cin >> tahun;
    for (int i = 0; i < jumlahBuku; i++) {
        if (daftarBuku[i].tahun == tahun) {
            cout << "Ditemukan: " << daftarBuku[i].judul << " (" << daftarBuku[i].tahun << ")\n";
            ditemukan = true;
        }
    }
    if (!ditemukan) {
        cout << "Buku dengan tahun terbit tersebut tidak ditemukan.\n";
    }
    return ditemukan ? 1 : 0;
}

int binarySearch(int tahun) {
    int kiri = 0;
    int kanan = jumlahBuku - 1;
    while (kiri <= kanan) {
        int tengah = (kiri + kanan) / 2;
        if (daftarBuku[tengah].tahun == tahun) {
            return tengah;
        } else if (daftarBuku[tengah].tahun < tahun) {
            kiri = tengah + 1;
        } else {
            kanan = tengah - 1;
        }
    }
    return -1;
}

int cariBinarySearch() {
    if (jumlahBuku == 0) {
        cout << "Data buku kosong.\n";
        return 0;
    }
    bubbleSort();
    int tahun;
    cout << "Masukkan tahun terbit yang dicari: ";
    cin >> tahun;
    int hasil = binarySearch(tahun);
    if (hasil != -1) {
        cout << "Ditemukan: " << daftarBuku[hasil].judul << " (" << daftarBuku[hasil].tahun << ")\n";
        return 1;
    } else {
        cout << "Buku dengan tahun terbit tersebut tidak ditemukan.\n";
        return 0;
    }
}

int main() {
    int pilihan;
    do {
        cout << "\n=== Menu ===\n";
        cout << "1. Input Data Buku\n";
        cout << "2. Tampilkan Data Buku\n";
        cout << "3. Urutkan Buku dengan Bubble Sort\n";
        cout << "4. Urutkan Buku dengan Quick Sort\n";
        cout << "5. Cari Buku dengan Sequential Search\n";
        cout << "6. Cari Buku dengan Binary Search\n";
        cout << "7. Keluar\n";
        cout << "Pilihan Anda: ";
        cin >> pilihan;

        switch (pilihan) {
            case 1: inputBuku(); break;
            case 2: tampilBuku(); break;
            case 3:
                cout << "\nData sebelum diurutkan:\n";
                tampilBuku();
                bubbleSort();
                cout << "\nData setelah diurutkan (Bubble Sort):\n";
                tampilBuku();
                break;
            case 4:
                cout << "\nData sebelum diurutkan:\n";
                tampilBuku();
                quickSort(0, jumlahBuku - 1);
                cout << "\nData setelah diurutkan (Quick Sort):\n";
                tampilBuku();
                break;
            case 5: sequentialSearch(); break;
            case 6: cariBinarySearch(); break;
            case 7: cout << "Program selesai.\n"; break;
            default: cout << "Pilihan tidak valid.\n"; break;
        }
    } while (pilihan != 7);

    return 0;
}
