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
      
      <!-- Main Content -->
      <div class="w-18/20 pl-8 py-8">
        <div class="flex justify-between  mb-4 items-center">
          <div class="flex items-center gap-8 align-center">
            <button class="px-6 py-2 bg-teal-600 rounded-2xl text-white">
              {{ '<' }}
            </button>
            <div>
              <h1 class="text-2xl font-bold">  Ruang Meeting</h1>
              <p class="text-gray-400">Ruang Meeting</p>
            </div>
          </div>
          <div>
            <button class="bg-teal-600 text-white px-4 py-2 rounded-lg" 
              @click.prevent="$router.push('/about')"
            >+ Pesan Ruangan</button>
          </div>
        </div>
        
        <div class="p-6 shadow-2xl border border-1 border-gray-200 ">
          <table id="myTable" class="w-full bg-white shadow-md rounded-lg overflow-hidden">
            <thead class="bg-gray-200">
              <tr>
                <th class="p-3 text-left">UNIT</th>
                <th class="p-3 text-left">RUANG MEETING</th>
                <th class="p-3 text-left">KAPASITAS</th>
                <th class="p-3 text-left">TANGGAL RAPAT</th>
                <th class="p-3 text-left">WAKTU</th>
                <th class="p-3 text-left">JUMLAH PESERTA</th>
                <th class="p-3 text-left">JENIS KONSUMSI</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(meeting, index) in meetings" :key="index" class="">
                <td class="p-4 font-semibold">{{ meeting.unit }}</td>
                <td class="p-4">{{ meeting.ruangMeeting }}</td>
                <td class="p-4">{{ meeting.kapasitas }}</td>
                <td class="p-4">{{ meeting.tanggalRapat }}</td>
                <td class="p-4">{{ meeting.waktuMulai }} s/d {{ meeting.waktuSelesai }}</td>
                <td class="p-4">{{ meeting.jumlahPeserta }}</td>
                <td class="p-4">
                   {{meeting.jenisKonsumsi?.join(',')}}
                </td>
                
              </tr>
            </tbody>
          </table>
        </div>
        
        <!-- <div class="flex justify-end mt-4 space-x-2">
          <button class="px-3 py-1 bg-gray-300 rounded">Previous</button>
          <button class="px-3 py-1 bg-teal-600 text-white rounded">1</button>
          <button class="px-3 py-1 bg-gray-300 rounded">Next</button>
        </div> -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      meetings: []
    };
  },
  mounted() {
    this.fetchMeetings();
   
  },
  methods: {
    filteredItems(obj) {
      return Object.fromEntries(
        Object.entries(obj).filter(([_, value]) => value)
      );
    },
    async fetchMeetings() {
      try {
        const response = await axios.get('http://localhost:5000/api/meetings');
        this.meetings = response.data;

        console.log('meetings', this.meetings);
        this.$nextTick(function () {
        $('#myTable').DataTable();
        });
      } catch (error) {
        console.error('Error fetching meetings:', error);
      }
    },

  }
};
</script>

<style scoped>
  table {
    border-collapse: collapse;
    width: 100%;
  }
  th, td {
    text-align: left;
    padding: 8px;
  }
</style>
