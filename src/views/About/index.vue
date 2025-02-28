<template>
  <div class="flex flex-col h-screen">
    <!-- Header -->
    <header class="bg-gradient-to-r px-10 from-gray-900 to-gray-600 text-white flex justify-between items-center p-2 shadow-md">
      <div @click.prevent="$router.push('/')" class="text-xl font-bold "> <span class="italic"> FTL </span> <span class="ml-6">IMeeting</span></div>
      <div class="flex items-center space-x-4">
        <span class="text-sm">John Doe</span>
        <!-- <img src="https://via.placeholder.com/40" alt="Profile" class="w-10 h-10 rounded-full"> -->
      </div>
    </header>

    <div class="flex flex-1">
      <!-- Sidebar -->
      <div class="w-1/20 bg-white  pt-8  text-white mt-10 shadow-2xl  flex justify-center border border-gray-200 shadow-t-lg">
        <ul class="flex flex-col">
          <li class="mb-2 px-2 bg-teal-600 py-2 px-4 rounded"><a href="#" class=" hover:text-teal-400  text-white">
                  <!-- Home Icon -->
          <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
            <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8h5z"/>
          </svg>
          </a></li>
          <li class="mb-2 py-2 px-4  "><a href="#" class="text-indigo-950 hover:text-teal-400">
            <svg class="w-6 h-6 stroke-teal-400 text-white"  width="24" height="24" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
              <path d="M12 12c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm0 2c-3.33 0-10 1.67-10 5v3h20v-3c0-3.33-6.67-5-10-5z"/>
            </svg>
          </a></li>
        </ul>
      </div>
      <div class="w-7/8 p-6">
        <div id="app" class="w-10/12 mx-auto p-6 bg-white shadow-md mt-10 rounded-lg">
          <div class="flex items-center mb-6">
            <button class="text-gray-600 text-2xl mr-4" @click="goBack">
              <i class="fas fa-arrow-left"></i>
            </button>
            <div>
              <h1 class="text-2xl font-semibold">Ruang Meeting</h1>
              <p class="text-gray-500">Ruang Meeting > Pesan Ruangan</p>
            </div>
          </div>
          <div>
            <h2 class="text-xl font-semibold mb-4">Informasi Ruang Meeting</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
              <div>
                <label class="block text-gray-700">Unit</label>
                <select v-model="unit" class="w-full mt-1 p-2 border border-gray-300 rounded">
                  <option v-for="u in units" :key="u.id" :value="u.id">{{ u.name }}</option>
                </select>
              </div>
              <div>
                <label class="block text-gray-700">Ruang Meeting</label>
                <select v-model="ruangMeeting" class="w-full mt-1 p-2 border border-gray-300 rounded">
                  <option v-for="r in rooms" :key="r.id" :value="r.id">{{ r.name }}</option>
                </select>
              </div>
              <div class="col-span-1 md:col-span-2">
                <label class="block text-gray-700">Kapasitas</label>
                <input type="text" v-model="kapasitas" class="w-full mt-1 p-2 border border-gray-300 rounded" placeholder="Kapasitas Ruangan" disabled>
              </div>
            </div>
            <h2 class="text-xl font-semibold mb-4">Informasi Rapat</h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
              <div>
                <label class="block text-gray-700">Tanggal Rapat*</label>
                <input type="date" v-model="tanggalRapat" class="w-full mt-1 p-2 border border-gray-300 rounded">
              </div>
              <div>
                <label class="block text-gray-700">Waktu Mulai</label>
                <select v-model="waktuMulai" class="w-full mt-1 p-2 border border-gray-300 rounded">
                  <option v-for="w in waktuOptions" :key="w" :value="w">{{ w }}</option>
                </select>
              </div>
              <div>
                <label class="block text-gray-700">Waktu Selesai</label>
                <select v-model="waktuSelesai" class="w-full mt-1 p-2 border border-gray-300 rounded">
                  <option v-for="w in waktuOptions" :key="w" :value="w">{{ w }}</option>
                </select>
              </div>
              <div class="col-span-1 md:col-span-3">
                <label class="block text-gray-700">Jumlah Peserta</label>
                <input type="number" v-model="jumlahPeserta" class="w-full mt-1 p-2 border border-gray-300 rounded" placeholder="Masukan Jumlah Peserta">
              </div>
            </div>
            <h2 class="text-xl font-semibold mb-4">Jenis Konsumsi</h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
              <label class="block text-gray-700">
                <input type="checkbox" v-model="jenisKonsumsi.snackSiang" class="mr-2"> Snack Siang
              </label>
              <label class="block text-gray-700">
                <input type="checkbox" v-model="jenisKonsumsi.makanSiang" class="mr-2"> Makan Siang
              </label>
              <label class="block text-gray-700">
                <input type="checkbox" v-model="jenisKonsumsi.snackSore" class="mr-2"> Snack Sore
              </label>
            </div>
            <div class="flex justify-end">
              <button class="bg-red-500 text-white px-4 py-2 rounded mr-2" @click.prevent="batal">Batal</button>
              <button class="bg-teal-500 text-white px-4 py-2 rounded cursor-pointer" @click.prevent="simpan">Simpan</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      unit: '',
      ruangMeeting: '',
      kapasitas: '10 orang',
      tanggalRapat: '',
      waktuMulai: '',
      waktuSelesai: '',
      jumlahPeserta: '',
      jenisKonsumsi: {
        snackSiang: false,
        makanSiang: false,
        snackSore: false
      },
      units: [{ id: 1, name: 'Unit A' }, { id: 2, name: 'Unit B' }],
      rooms: [{ id: 1, name: 'Ruang 101' }, { id: 2, name: 'Ruang 102' }],
      waktuOptions: ['08:00', '10:00', '12:00', '14:00']
    };
  },
  methods: {
    simpan() {

      let jenisKonsumsiPay = []
  
      if(this.jenisKonsumsi.snackSiang)  jenisKonsumsiPay.push('snack siang')
      if(this.jenisKonsumsi.makanSiang)  jenisKonsumsiPay.push('makan siang') 
      if(this.jenisKonsumsi.snackSore)  jenisKonsumsiPay.push('Snack Sore' )



      axios.post('http://localhost:5000/api/meetings', {
        unit: "Unit Keuangan",
        ruangMeeting: this.ruangMeeting,
        tanggalRapat: this.tanggalRapat,
        waktuMulai: this.waktuMulai,
        waktuSelesai: this.waktuSelesai,
        jumlahPeserta: this.jumlahPeserta,
        jenisKonsumsi: jenisKonsumsiPay
      }).then(response => {
        alert('Pemesanan berhasil!');
        this.$router.push('/')
      }).catch(error => {
        console.error('Error:', error);
      });
    }
  }
};
</script>
