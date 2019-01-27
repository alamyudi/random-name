<template>
  <b-container fluid class="w-100 h-100">
    <b-row class="top-row" ref="memberContainer">
      <div class="rounded-circle" v-for="member in members" :key="member.id" 
      :style="{
        backgroundColor: randomColor(member.id), 
        top: member.top + 'px', 
        left: member.left + 'px',
        borderColor: member.choosed ? member.borderColor : 'none',
        borderStyle: member.choosed ? 'solid' : 'none',
        borderWidth: member.choosed ? '3px' : '0px',
        }">
        <span class="member-name">
          {{member.name}}
        </span>
      </div>
    </b-row>
    <b-row class="justify-content-md-center bottom-row " align-v="end">
        <b-button-group size="lg">
          <b-button variant="warning" @click="onRandomTextClick">Who's Next</b-button>
          <!-- <b-button variant="danger" @click="onResetClick">Reset</b-button> -->
        </b-button-group>
    </b-row>
  </b-container>
</template>

<script>

const members = [
  { // Agung
    id: 1,
    name: 'AA',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Alam
    id: 2,
    name: 'AYA',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Boby
    id: 3,
    name: 'BS',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Devi
    id: 4,
    name: 'DL',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Fajar
    id: 5,
    name: 'FSM',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Jeffry
    id: 6,
    name: 'JA',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Kurnia
    id: 7,
    name: 'KJE',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Maulana
    id: 8,
    name: 'YM',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Panji
    id: 9,
    name: 'PYW',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Puguh
    id: 10,
    name: 'TPS',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Ricky
    id: 11,
    name: 'RH',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Ridwan
    id: 12,
    name: 'RFS',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Yanti
    id: 13,
    name: 'YS',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  },
  { // Yusuf
    id: 14,
    name: 'MY',
    top: '0',
    left: '0',
    choosed: false,
    borderColor: '#f0f0f0',
  }
]

export default {
  name: 'app',
  data: function() {
    return  {
      members,
      random: false,
      randomName: null,
      colorCache: {}
    }
  },
  mounted: function() {
    this.updateMemberPosition()
  },
  methods: {
    onRandomTextClick: function () {
      this.updateMemberPosition()
    },
    onResetClick: function() {
    },
    randomColor(id) {
      const r = () => Math.floor(256 * Math.random());
      return this.colorCache[id] || (this.colorCache[id] = `rgb(${r()}, ${r()}, ${r()})`);
    },
    makeNewPosition(){
      // Get viewport dimensions (remove the dimension of the div)
      var h = window.innerHeight - 200;
      var w = window.innerWidth - 70;
      
      var nh = Math.floor(Math.random() * h);
      var nw = Math.floor(Math.random() * w);
      
      return [nh,nw];
    },
    updateMemberPosition() {
      if (this.random == false) {
        this.random = true
        this.setChosenMemberBorderColor()
        this.randomName = setInterval( () => {
            this.members.forEach((val) => {
              var position = this.makeNewPosition()
              val.top = position[0].toString()
              val.left = position[1].toString()
            })
        }, 1100)
      }else {
        this.random = false
        clearInterval(this.randomName);
        const availableMember = this.members.filter(member => member.choosed == false);
        const memberSelected = availableMember[Math.floor(Math.random()*availableMember.length)]
        const chosenIndex = this.members.findIndex((member) => {
          return member.id === memberSelected.id
        })
        const selectedName = this.members[chosenIndex]
        selectedName.choosed = true
        selectedName.borderColor = '#42f477'
        this.moveSelectedNameToWinnerPlace(selectedName)
      }
    },
    setChosenMemberBorderColor(){
      this.members.forEach((val) => {
        if (val.choosed === true) {
          val.borderColor = '#f45641'
        }
      })
    },
    moveSelectedNameToWinnerPlace(member) {
      const memberContainer = this.$refs.memberContainer
      const leftPosition = ( memberContainer.clientWidth / 2 ) - 37
      const topPosition = memberContainer.clientHeight - 120
      member.top = topPosition.toString()
      member.left = leftPosition.toString()
    }
  }
}
</script>

<style>
html,body {
    height: 100%;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.left-container {
  background: white;
  margin-left: -15px;
}
.top-row {
  height: 90% !important;
  background: #f0f0f0;
}
.bottom-row {
  background: #2c3e50;
  height: 10% !important;
  padding-bottom: 20px;
}
.rounded-circle {
  position: fixed;
  width: 70px;
  height: 70px;
  margin: 6px;
  background-color: #555;
  text-align: center;
  vertical-align: -webkit-baseline-middle;
  line-height:65px;
  font-size: 30px;
  transition: all 1s ease-in-out;
}
.member-name{
  font-weight: 200;
  color: rgb(255, 255, 255);
}
</style>
