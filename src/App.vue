
<template>
  <div id="app" class="container">
    <label for="enter" :class="{ red: err, green: success}">Введите название поля от A1 до H8</label>
    <div class="row">
    <input type="text" v-model="field" class="form-control col-1">
    <span :class="{dis: !err, 'col-4': true}">Такого поля нет</span>
    </div>
    <p :class="{green: !success}">{{res}}</p>
  </div>
</template>

<script>

export default {
  data () {
    return {
      field: '',
      res: '',
      err: false,
      success: false,
    }
  },
  methods:{// функция возвращающая нужные нам значения
    Result: function(num, symbol){
      let fields = [];

      // две функции принимающие для преобразования буквы в координату и обратно
      function number(sym){// делает из буквы координату
        let letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'];
        return letters.indexOf(sym) + 1
      }
      function letter(num){// делает из координаты букву
        let letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'];
        if (num < 0 || num > 8) {
          return ''
        }
        for(let i = 0; i < 8; i++){
          if (num == (i + 1)) {
            return letters[i]
          }
        }
      }

      fields.push({x:(+num + 2), y:(letter(number(symbol) + 1))})
      fields.push({x:(+num + 2), y:(letter(number(symbol) - 1))})
      fields.push({x:(+num - 2), y:(letter(number(symbol) + 1))})
      fields.push({x:(+num - 2), y:(letter(number(symbol) - 1))})
      fields.push({x:(+num + 1), y:(letter(number(symbol) + 2))})
      fields.push({x:(+num + 1), y:(letter(number(symbol) - 2))})
      fields.push({x:(+num - 1), y:(letter(number(symbol) + 2))})
      fields.push({x:(+num - 1), y:(letter(number(symbol) - 2))})
      
      fields = fields.filter(field => {
        if (field.x > 0 && field.x != NaN && field.x < 8 && field.y != undefined && field.y != '') {
          return true
        }
      })

      let result = fields.map(item => {
        return item.y + item.x
      })

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
  },
  watch: {
    // это валидация:
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

            this.res = this.field;
            this.Success();
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
  }
}
</script>

<style lang="scss">
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
    .green{
      color: green;
      border-color: green;
      transform: scale(0)
    }
    .dis{
      transform: scale(0);
      overflow: hidden;
    }
  }
</style>
