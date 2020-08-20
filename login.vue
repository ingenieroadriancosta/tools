<!-- ADRIAN COSTA -->
<template>
  <v-container>
      
    <v-app-bar app color="white">
      <div class="d-flex align-center">
        <v-img
          alt="Logo"
          class="shrink mr-2"
          contain
          src="https://raw.githubusercontent.com/ingenieroadriancosta/tools/master/itus.png"
          transition="scale-transition"
          width="40"
        />
      </div>
      <v-toolbar-title>ITUS </v-toolbar-title>
        <v-divider vertical></v-divider>
    <v-toolbar-items >
      <v-divider vertical></v-divider>
      <v-btn text @click="clilog">
        {{logtit}}
      </v-btn>
      <v-divider vertical></v-divider>
    </v-toolbar-items>
        </v-app-bar>
        <div class="mt-1" v-show="!logshow">
            <h2>Principal<v-icon  right>mdi-home</v-icon></h2>
            <v-toolbar
                    dark
                    color="teal"
                >
                <v-text-field
                v-model="selecttext"
                @keyup="tableord"
                cache-items
                class="mx-4"
                flat
                hide-no-data
                hide-details
                label="Presione ENTER para Buscar un email"
                solo-inverted
                ></v-text-field>
            </v-toolbar>
            <!-- -->
            <!-- -->
            <!-- -->
            <!-- -->
            <v-data-table
                :headers="headers"
                :items="desserts"
                :items-per-page="5"
                class="elevation-1"
            ></v-data-table>
            <!-- -->
            <!-- -->
            <!-- -->
            <!-- -->
        </div>
      










      <div class="mt-1" v-show="logshow">
          <h2>Login</h2>
        <v-col cols="7" sm="6" md="3" id="email">
          <v-text-field
            label="email"
            outlined
            value="eve.holt@reqres.in"
            :rules='emailRules'
            v-model="fpost.email"
          ></v-text-field>
        </v-col>
        <v-col cols="7" sm="6" md="3" id="password">
          <v-text-field
            label="contraseña es pistol, blanco da error"
            outlined
            value="pistol"
            :rules='loginRules'
            v-model="fpost.password"
          ></v-text-field>
        </v-col>
        <v-btn color="error"  @click="sendpost">
          Ingresar
        </v-btn>
      </div>

    



      <v-dialog v-model="dialoginfo" persistent>
        <v-card>
          <v-card-title>
        <v-icon v-show="iconerr" color="blue" dark right large>mdi-checkbox-marked-circle</v-icon>
        <v-icon v-show="!iconerr" color="red" dark right large>mdi-cancel</v-icon>
            {{msgtit}}
          </v-card-title>
          <v-card-text v-html="msgtext">
            {{msgtext}}
          </v-card-text>
          <v-btn color="color_b"  @click="clsmsg" v-show="!bloqb">
          Cerrar
          </v-btn>
        </v-card>
      </v-dialog>

  </v-container>
</template>

<script>
  export default {
    data(){
      return{
          logtit:"Login",
        dialoginfo:false,
        iconerr:true,
        logshow:true,
        color_b:'blue',
        bloqb:false,
        msgtit:"",
        msgtext:"",
        ntimesfailed:0,
        nsegs:0,
        loginexitoso:false,
        datin:Date.now(),
        datout:Date.now(),
        inter:[],
        fpost: {
            email:"eve.holt@reqres.in",
            password:"pistol",
        },
        emailRules: [
                v => !!v || "E-mail is required",
                v => /.+@.+/.test(v) || "E-mail must be valid"
            ],
        loginRules: [v => !!v || "The input is required"],

        selecttext:"",

        headers: [
          {
            text: 'Id',
            align: 'start',
            sortable: true,
            value: 'id',
          },
          { text: 'email', value: 'email',sortable: false, },
          { text: 'First name', value: 'first_name',sortable: false, },
          { text: 'Last name', value: 'last_name',sortable: false, },
          { text: 'Avatar', value: 'avatar',sortable: false, },
        ],
        desserts: [
          {
            id: 1,
            email: 159,
            first_name: 6.0,
            last_name: 24,
          },
          {
            id: 2,
            email: 237,
            first_name: 9.0,
            last_name: 37,
          },
        ],
        dessertsc: [],
        emailmatch:""
        
      }
    },
    
    methods: {
      fnc0: function (event) {
          if( this.logshow ){
              this.logshow = false;
          }else{
              this.logshow = true
          }
      },
      clilog: function () {
          this.logtit="Login"
          this.logshow=true
          if( this.loginexitoso ){
              this.loginexitoso=false
              this.datout=new Date(Date.now())
              const millis = (this.datout - this.datin)/1000;
              this.infomsg("LogOut", "Gracias [" + 
                    this.fpost.email + 
                    "],<br/> su sesión con una duración de "+
                    millis +" segundos"+
                    " ha terminado. <br/>Hora de inicio: ["+
                    this.datin.getHours()+":"+
                    this.datin.getMinutes()+":"+
                    this.datout.getSeconds()+
                    "] <br/>Hora de fin [" +
                    this.datout.getHours()+":"+
                    this.datout.getMinutes()+":"+
                    this.datout.getSeconds()+
                    "]")
          }
      },
      infomsg: function (title, msg) {
          this.iconerr = true;
          this.color_b='blue'
          this.bloqb=false
          this.dialoginfo = true
          this.msgtit = title
          this.msgtext = msg
      },
      errormsg: function (title, msg) {
          this.iconerr = false;
          this.color_b='red'
          this.bloqb=false
          this.dialoginfo = true
          this.msgtit = title
          this.msgtext = msg
      },
      clsmsg: function (event) {
          this.dialoginfo = false;
      },
      ntimfnc: function (event) {
          this.ntimesfailed++;
          return this.ntimesfailed;
      },
      ntimzero: function (event) {
          this.ntimesfailed = 0;
          return this.ntimesfailed;
      },
      timero:function(){
          this.nsegs++;
          this.msgtext = "Error al ingresar, espere, "+this.nsegs+" segundos de 300"
          if( this.nsegs>=300 ){
              clearInterval(this.inter)
              this.clsmsg()
          }
      },


        tableord:function(rejson){
            var dat = rejson['data'];
            this.desserts=[]
            this.dessertsc.forEach(element => {
                if( element['email'].includes(this.selecttext) ){
                    this.desserts.push(element)
                }
            });
        },


        tableresponse:function(rejson){
            var dat = rejson['data'];
            this.desserts=[]
            this.dessertsc=[]
            dat.forEach(element => {
                this.desserts.push(element)
                this.dessertsc.push(element)
            });
        },



        filltable:function(){
            var tbres = this.tableresponse;
            var xhr = new XMLHttpRequest();
            xhr.open("GET", "https://reqres.in/api/users?page=1", true);
            xhr.setRequestHeader('Content-Type', 'application/json');
            xhr.send(null);
            xhr.onreadystatechange = function () {
                if(this.readyState!=4){
                    return;
                }
                if(this.status==200){
                    tbres( JSON.parse(this.responseText) );
                }else{
                    tbres( null );
                }
                return;
            };
        },







                /*
        headers: [
          {
            text: 'Id',
            align: 'start',
            sortable: true,
            value: 'id',
          },
          { text: 'email', value: 'email',sortable: false, },
          { text: 'First name', value: 'first_name',sortable: false, },
          { text: 'Last name', value: 'last_name',sortable: false, },
          { text: 'Avatar', value: 'avatar',sortable: false, },
        ],
        desserts: [
            }
            //*/
            









      onreadystatechange: function (readyState, status, responseText) {
          if (readyState != 4){
                return;
            }
            if (status == 400) {
                this.logtit="Login"
                this.loginexitoso=false
                this.errormsg( "Error", 
                        "Error al ingresar, lleva " + 
                        this.ntimfnc() + 
                        " intentos.<br/>"+
                        "credenciales no válidas con error:<br/>"+
                        status
                        );
                if(this.ntimesfailed>=3){
                    this.bloqb=true
                    this.nsegs = 0
                    this.inter = setInterval(this.timero, 1000);
                }
                return;
            }
            if (status == 200) {
                this.datin=new Date(Date.now())
                this.logtit="Logout"
                this.loginexitoso=true
                this.logshow=false
                this.ntimzero()
                this.infomsg( 
                    "En hora buena", 
                    "Bienvenido,"+"<br/>"+
                    this.fpost.email+"<br/>"+
                    " Se ha logueado con éxito a la plataforma"
                    )
                this.filltable();
            }
      },
      // 
    sendpost:function(){
        const onread = this.onreadystatechange;
        const cemail = this.fpost.email
        const cpassword = this.fpost.password
        var xhr = new XMLHttpRequest();
        xhr.open("POST", "https://reqres.in/api/login", true);
        xhr.setRequestHeader('Content-Type', 'application/json');
        xhr.send(JSON.stringify({
                email: cemail,
                password:cpassword,
        }));
        xhr.onreadystatechange = function () {
            onread( this.readyState, this.status, this.responseText );
            return;
        };
      },


    },
  }
</script>
