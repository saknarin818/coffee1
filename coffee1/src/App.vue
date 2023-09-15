<template>
  <div class="container mt-5">
    <h1 class="mb-4">ร้านกาแฟ</h1>

    <!-- แสดงรายชื่อร้านกาแฟ -->
    <div class="row">
      <div class="col-md-6">
        <h2>รายชื่อร้านกาแฟ</h2>
        <div class="card-group">
          <div
            v-for="(shop, index) in coffeeShops"
            :key="index"
            class="card mb-3"
          >
            <img
              :src="shop.imageUrl"
              class="card-img-top"
              alt="รูปร้านกาแฟ"
            >
            <div class="card-body">
              <h5 class="card-title">{{ shop.name }}</h5>
              <p class="card-text">{{ shop.description }}</p>
            </div>
            
          </div>
        </div>
      </div>

      <!-- แสดงข้อมูลการจอง -->
      <div class="col-md-6">
        <h2>ข้อมูลการจอง</h2>
        <ul class="list-group">
          <li
            v-for="(reservation, index) in reservations"
            :key="index"
            class="list-group-item"
          >
            <b>ชื่อร้าน:</b> {{ reservation.coffeeShopName }}<br>
            <b>ชื่อผู้จอง:</b> {{ reservation.customerName }}<br>
            <b>เบอร์โทร:</b> {{ reservation.phoneNumber }}<br>
            <b>วันที่:</b> {{ reservation.date }}<br>
            <b>เวลา:</b> {{ reservation.time }}<br>
            <b>จำนวนโต๊ะ:</b> {{ reservation.tableCount }}<br>
          </li>
        </ul>
      </div>
    </div>

    <!-- แบบฟอร์มจองโต๊ะ -->
    <div class="mt-4">
      <h2>จองโต๊ะ</h2>
      <form @submit.prevent="saveReservation">
        <div class="mb-3">
          <label for="coffeeShopSelect" class="form-label">เลือกร้านกาแฟ</label>
          <select v-model="currentCoffeeShopIndex" id="coffeeShopSelect" class="form-select">
            <option value="" disabled>กรุณาเลือกร้านกาแฟ</option>
            <option
              v-for="(shop, index) in coffeeShops"
              :key="index"
              :value="index"
            >
              {{ shop.name }}
            </option>
          </select>
        </div>
        <div class="mb-3">
          <label for="customerName" class="form-label">ชื่อผู้จอง</label>
          <input v-model="customerName" type="text" class="form-control" id="customerName">
        </div>
        <div class="mb-3">
          <label for="phoneNumber" class="form-label">เบอร์โทร</label>
          <input v-model="phoneNumber" type="text" pattern="[0-9]{10}" required class="form-control" id="phoneNumber">
        </div>
        <div class="mb-3">
          <label for="date" class="form-label">วันที่</label>
          <input v-model="date" type="date" class="form-control" id="date">
        </div>
        <div class="mb-3">
          <label for="time" class="form-label">เวลา</label>
          <input v-model="time" type="time" class="form-control" id="time">
        </div>
        <div class="mb-3">
          <label for="tableCount" class="form-label">จำนวนโต๊ะ</label>
          <input v-model="tableCount" type="number" class="form-control" id="tableCount" min="1">
        </div>
        <button type="submit" class="btn btn-primary">บันทึก</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      coffeeShops: [
        { name: 'ไปยานใหญ่ ', id: 1, imageUrl: 'https://scontent.fcnx2-1.fna.fbcdn.net/v/t39.30808-6/326435697_501993212005268_2267045348590722770_n.jpg?_nc_cat=103&ccb=1-7&_nc_sid=a2f6c7&_nc_eui2=AeH0Q4qXNrxJVT54T0cFNC1qxTbvRg4F5HPFNu9GDgXkc81nD3XBuj1ViRYkv6a4ko_BkUgAVzvK9WVhgVuzBefR&_nc_ohc=yOx99RPDDD0AX9g7swg&_nc_ht=scontent.fcnx2-1.fna&oh=00_AfCqTU6dm8YiEsBo7kH3GbMy7YosWzYz-Ur-TkQ7czXBHw&oe=6508A1CF', description: '26/1 หมู่ 9 ตำบลหนองหาร อำเภอสันทราย, Chiang Mai, Thailand, Chiang Mai' },
        { name: 'TAMMA CAFE ', id: 2, imageUrl: 'https://scontent.fcnx2-1.fna.fbcdn.net/v/t39.30808-6/361305041_779199773993037_7111305937619716444_n.jpg?_nc_cat=110&ccb=1-7&_nc_sid=a2f6c7&_nc_eui2=AeFxvmteBvy2K79UsDn0RYOZKhovrXyAa8YqGi-tfIBrxpKuFLPrFlvZOGn0EPhZHElf6G2dL5UlEl78fU6V5jXx&_nc_ohc=uHWrPKKsU5IAX-bQClg&_nc_ht=scontent.fcnx2-1.fna&oh=00_AfCldRePkm9xyR75WX2AT0E6PCwTgLjqStyApa5lrnLYyQ&oe=6508E387', description: '648 หมู่ 5 ตำบลหนองหาร อำเภอสันทราย, Amphoe San Sai, Thailand, Chiang Mai' },
        { name: 'ตะวันแดง', id: 3, imageUrl: 'https://scontent.fcnx2-1.fna.fbcdn.net/v/t39.30808-1/357020255_652721053558857_528736678354773129_n.jpg?stp=dst-jpg_p320x320&_nc_cat=105&ccb=1-7&_nc_sid=754033&_nc_eui2=AeFEA72Z2_JUgxLfwAkTB7-B39Zl0-plB1rf1mXT6mUHWshg6om9d-XCUPRwSdAt6zJ6HTJk7MlFcIA2ri34EPSY&_nc_ohc=8vKI-7pmMGkAX-j3e3P&_nc_ht=scontent.fcnx2-1.fna&oh=00_AfAenfvOQf59aI6ZxqlVF3Sf1n3TmUo8FHowpgL3B0yVHw&oe=650993B5', description: '73 ถนนคลองชลประทาน ตำบลสุเทพ อำเภอเมืองเชียงใหม่ เชียงใหม่ 50200' },
      ],
      reservations: [],
      currentCoffeeShopIndex: null,
      customerName: '',
      phoneNumber: '',
      date: '',
      time: '',
      tableCount: 1,
    };
  },
  methods: {
    reserveTable(coffeeShopIndex) {
      this.currentCoffeeShopIndex = coffeeShopIndex;
    },
    saveReservation() {
      if (this.currentCoffeeShopIndex !== null) {
        this.reservations.push({
          coffeeShopName: this.coffeeShops[this.currentCoffeeShopIndex].name,
          customerName: this.customerName,
          phoneNumber: this.phoneNumber,
          date: this.date,
          time: this.time,
          tableCount: this.tableCount,
        });

        // Reset form data
        this.customerName = '';
        this.phoneNumber = '';
        this.date = '';
        this.time = '';
        this.tableCount = 1;
        this.currentCoffeeShopIndex = null;
      }
    },
  },
};
</script>

<style>
.coffee-image {
  max-width: 80px;
  border: 2px solid #FF5733;
  border-radius: 10px;
  padding: 5px;
}
</style>
