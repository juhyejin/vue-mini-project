<script setup lang="ts">
import AtomButton from "@/components/Atom/button/AtomButton.vue";



</script>


<template>
  <main class="calender-container">
    <div class="calender-header">
      <AtomButton>이전달</AtomButton>
      <div class="calender-title">2023년 05월</div>
      <AtomButton>다음달</AtomButton>
    </div>
    <div class="calender-body">
      <div class="days day">1</div>
      <div class="days day">2</div>
      <div class="days day">3</div>
      <div class="days day">4</div>
      <div class="days day">5</div>
      <div class="days day">6</div>
      <div class="days day">7</div>
      <div class="days day">8</div>
      <div class="days day">9</div>
      <div class="days day">10</div>
      <div class="days day">11</div>
      <div class="days day">12</div>
      <div class="days day">13</div>
      <div class="days day">14</div>
      <div class="days day">15</div>
      <div class="days day">16</div>
      <div class="days day">17</div>
      <div class="days day">18</div>
      <div class="days day">19</div>
      <div class="days day">20</div>
      <div class="days day">21</div>
      <div class="days day">22</div>
      <div class="days day">23</div>
      <div class="days day">24</div>
      <div class="days day">25</div>
      <div class="days day">26</div>
      <div class="days day">27</div>
      <div class="days day">28</div>
      <div class="days day">29
        <div class="bg-pink">g2</div>
        <div class="bg-pink">g2</div>
        <div class="bg-pink">g2</div>
        <div class="bg-pink">g2</div>
        그외 4개
      </div>
      <div class="days day">30</div>
      <div class="days day">31</div>
      <div class="days day"></div>
      <div class="days day"></div>
      <div class="days day"></div>
      <div class="days day"></div>
    </div>
  </main>
</template>

<style>
.calender-container{
  height: 85vh;
  background: #f2f2f2;
  color: #181818;
  padding: 10px;
  display: flex;
  flex-direction: column;
  gap:10px;
}
.calender-header{
  display: flex;
  justify-content: center;
  align-self: center;
}
.calender-body {
  flex-grow: 1;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 5px;
  grid-auto-rows: 1fr;
}
.days{
  border: 1px solid #2c3e50;
  border-radius: 8px;
}
.bg-pink{
  background: pink;
}
</style>
