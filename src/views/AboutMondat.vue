<template>
  <div class="about">
    <VSelect :items="computedUsers" item-text="name" v-model="selectedUser" placeholder="Пользователь" return-object/>
<!--    <div :key="userInd" v-for="(user, userInd) in selectedUser">-->
    <div v-if="!!selectedUser">
      <h2>{{ selectedUser.name }}</h2>

      <VSelect :items="selectedUser.objects" item-text="name" v-model="selectedObject" placeholder="Файл" return-object/>
      <div v-if="selectedObject">
        {{ selectedObject.name }}
<!--        - {{ selectedObject.rules }}-->
<!--        - {{ selectedObject.rules | rule }}-->
        <v-row>
          <v-col>
<!--            {{!(selectedObject.rules & 4)}}-->
            <v-btn :disabled="!(selectedObject.rules & 4)">Читать</v-btn>
          </v-col>
          <v-col>
<!--            {{!(selectedObject.rules & 2)}}-->
            <v-btn :disabled="!(selectedObject.rules & 2)">Записать Изменения</v-btn>
          </v-col>
<!--          <v-col>-->
<!--           {{!(selectedObject.rules & 1)}}-->
<!--            <v-btn :disabled="!(selectedObject.rules & 1)">Передать права</v-btn>-->
<!--          </v-col>-->
        </v-row>
      </div>
    </div>
    <!--      </div>-->
  </div>
</template>

<script>
var userNames = [
  "Нео",
  "Морфиус",
  "Тринити",
  "Смит",
  "Оракул",
  "Архитектор"
]

var fileNames = [
  "Файл по имени Нео",
  "Файл по имени Морфиус",
  "Файл по имени Тринити",
  "Файл по имени Смит",
  "Файл по имени Оракул",
  "Файл по имени Архитектор"
]


var users = []
for (let i = 0; i < 3; i++) {
  users.push({name: userNames[Math.floor(Math.random()*userNames.length)], id: i, rules: Math.floor(Math.random()*7)})
}

var objects = []
for (let i = 0; i < 5; i++) {
  objects.push({id: i, name: fileNames[Math.floor(Math.random()*fileNames.length)], rules: Math.floor(Math.random()*7)})
}

export default {
  name: "About",
  data() {
    return {
      selectedUser: null,
      selectedObject: null,
      userLen: 3,
      objectLen: 5
    }
  },
  filters: {
    rule(val) {
      switch (val) {
        case 0: return "Полный запрет"
        case 1: return "Передача прав"
        case 2: return "Запись"
        case 3: return "Запись, Передача прав"
        case 4: return "Чтение"
        case 5: return "Чтение, Передача прав"
        case 6: return "Чтение, Запись"
        case 7: return "Полный доступ"
      }
    }
  },
  computed:{
  }
}
</script>