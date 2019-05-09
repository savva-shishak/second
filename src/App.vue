<template>
  <!--Не интересный шаблон-->
  <div id="app" class="container">
    <label for="enter" :class="{ red: err, dis: success}">Введите название поля от A1 до H8</label>
    <div class="row">
    <input type="text" v-model="field" class="form-control col-1">
    <span :class="{dis: !err, 'col-4': true}">Такого поля нет</span>
    </div>
    <p :class="{dis: !success}">{{res}}</p>
  </div>
</template>

<script>
// сам ход работы:
// для того чтобы определять доступные для коня ходы я решил перевести все названия клеток в координаты
export default {
  data () {
    return {
      // для самой задачи
      field: '',
      res: '',
      // для стилей
      err: false,
      success: false,
    }
  },
  watch: {// это валидация, чтобы не получить не существующие поля:
    field: function() {
      if (this.field.length > 2) {// проверка на количество знаков
        this.Error();
      } else {
        this.err = false
        if (this.field.length == 2) {// когда пройдена проверка на количество знаков, проверяем сами знаки

          let symbol = this.field.toLowerCase().charAt(0),
          num = this.field.charAt(1),
          test1 = /[a-h]/,
          test2 = /[1-8]/;
          if((test1.test(symbol)) && (test2.test(num))){

            this.Result(num, symbol);// при успешой валидации используем нужную функцию

          } else {

            this.Error();
          };

        } else {
          this.res = ''
          this.None()
        }
      }
    }
  },
  methods:{// функция показывающая результат
    Result: function(num, symbol){
      this.Success();// стилями говорим что всё успешно

      let fields = [];// создаём локальный массив

      // две функции принимающие для преобразования буквы в ординаты и обратно
      function number(sym){
        let letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'];
        return letters.indexOf(sym) + 1// делает из буквы ординату
      }
      function letter(num){
        let letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'];
        if (num < 0 || num > 8) {// делает из ординаты букву
          return ''
        }
        for(let i = 0; i < 8; i++){
          if (num == (i + 1)) {
            return letters[i]
          }
        }
      }

      // заполняем массив всеми координатами
      fields.push({x:(+num + 2), y:(letter(number(symbol) + 1))})
      fields.push({x:(+num + 2), y:(letter(number(symbol) - 1))})
      fields.push({x:(+num - 2), y:(letter(number(symbol) + 1))})
      fields.push({x:(+num - 2), y:(letter(number(symbol) - 1))})
      fields.push({x:(+num + 1), y:(letter(number(symbol) + 2))})
      fields.push({x:(+num + 1), y:(letter(number(symbol) - 2))})
      fields.push({x:(+num - 1), y:(letter(number(symbol) + 2))})
      fields.push({x:(+num - 1), y:(letter(number(symbol) - 2))})
      
      // фильтруем от невозможных координат
      fields = fields.filter(field => {
        if (field.x > 0 && field.x != NaN && field.x < 8 && field.y != undefined && field.y != '') {
          return true
        }
      })

      // делает массив с обычными названиями координат
      let result = fields.map(item => {
        return item.y + item.x
      })

      // вот и всё!
      this.res = 'Конь может сходить на: ' + result.join(', ');
    },

    // определяем 3 состояния надписи:
    // когда значение не прошло валидацию
    Error: function() {
      this.err = true;
      this.success = false;
    },
    // когда прошло валидацию
    Success: function() {
      this.err = false;
      this.success = true;
    },
    // когда значение ещё недописано
    None: function(){
      this.err = false;
      this.success = false;
    }
  }
}
</script>

<style lang="scss">
// не много стилей для красоты
*{
  transition: .3s
}
  #app{
    padding-top: 70px;
    label{
      padding: 3px; 
      border: 1px solid white
    }
    .red{
      color: red;
      border-color: red; 
    }
    .dis{
      transform: scale(0)
    }
  }
</style>
