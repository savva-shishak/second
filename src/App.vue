
<template>
  <div id="app" class="container">
    <!-- привязываем переменные к формам через атрибут v-model, а также используем событие клика на кнопку enter + немножечко bootstrap стилей-->
    <input type="number" v-model="num1" @keyup.enter='alertSum' class="form-control col-3">
    <input type="number" v-model="num2" @keyup.enter='alertSum' class="form-control col-3">
    <!--Кнопка для -->
    <button class="btn btn-primary" @click="alertSum">Сложить</button>
  </div>
</template>

<script>
// объявляем две переменные со значениями, указанными в условиях
export default {
  data () {
    return {
      num1: 0.1,
      num2: 0.2
    }
  },// а также метод, который выводит их сумму при клике на кнопку или нажатии клавиши enter
  methods:{
    alertSum: function(){
      // вывод переменных через уведомление, нужно к каждой переменной подставлять знак "+", иначе, при изменении значений, первая воспримется как строка
      alert(+this.num1 + +this.num2)
    }
  }
}
</script>

<style lang="scss">
// не много scss стилей, чтобы приятней было посмотреть
  #app{
    padding-top: 50px;
    input.form-control{
      margin-bottom: 20px
    }
  }
</style>
