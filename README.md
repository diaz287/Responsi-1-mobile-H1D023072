# Responsi-1-mobile-H1D023072

Nama : Khafriza Diaz Permana
NIM : H1D023072
Shift KRS : F
Shift sekarang : D

# Video Demo

https://github.com/user-attachments/assets/f12e7f21-9fc9-4d82-9415-cc9c67165f9f

# Penjelasan alur 
Berikut adalah alur data aplikasi, mulai dari pemanggilan API hingga data disajikan di layar pengguna:

1. Pemanggilan API (Network Request)
Inisiasi (Activity): Ketika pengguna membuka CoachActivity atau SquadActivity, Activity tersebut akan memanggil fungsi viewModel.fetchTeamDetails(TEAM_ID) yang ada di MainViewModel. TEAM_ID yang digunakan adalah 342 (Derby County).

ViewModel: MainViewModel menerima permintaan ini dan meluncurkan coroutine baru menggunakan viewModelScope.launch.

Retrofit Service: Di dalam coroutine, ViewModel memanggil RetrofitInstance.api.getTeamDetails(). Panggilan ini sudah dikonfigurasi untuk menyertakan AUTH_TOKEN ("074a5e886c10417d804dfa7db1853c7d") sebagai Header X-Auth-Token.

2. Pengolahan Data (Data Processing)
Parsing (GSON): API football-data.org mengembalikan respons dalam format JSON. Retrofit, yang dikonfigurasi dengan GsonConverterFactory, secara otomatis mem-parsing data JSON ini ke dalam data class Kotlin: SearchResponse. SearchResponse ini berisi objek Coach dan sebuah List<Player>.

LiveData: Setelah data berhasil didapat dan di-parsing, MainViewModel memperbarui nilai _teamDetails (sebuah MutableLiveData) dengan hasil SearchResponse tersebut.

3. Penyajian di Layar (UI Display)
Observing (Activity): CoachActivity dan SquadActivity mengobservasi (mengamati) viewModel.teamDetails (sebuah LiveData). Ketika nilai LiveData berubah, callback observe akan terpicu.

Menampilkan Data Pelatih: Di CoachActivity, observer mengambil data searchResponse?.coach dan langsung menggunakannya untuk mengisi TextViews (seperti tvCoachName, tvCoachBirthdate, dll).

Menampilkan Data Skuad (RecyclerView):

Di SquadActivity, observer mengambil data searchResponse?.squad (yaitu List<Player>).

List pemain ini kemudian diteruskan ke playerAdapter melalui fungsi playerAdapter.updateData(squad).

PlayerAdapter (sebuah RecyclerView.Adapter) akan memproses list ini dan menampilkan setiap pemain ke dalam item_player.xml.

Menampilkan Detail Pemain (Bottom Sheet):

Ketika pengguna mengklik salah satu item pemain di SquadActivity, sebuah click listener akan terpicu, memanggil fungsi showPlayerDetailsDialog(player).

Objek Player yang diklik (yang sudah mengimplementasikan Serializable) diteruskan sebagai argument ke PlayerDetailsBottomSheet.

PlayerDetailsBottomSheet kemudian mengambil data Player dari arguments dan menggunakannya untuk mengisi TextViews di dalam dialog (dialog_player_details.xml).
