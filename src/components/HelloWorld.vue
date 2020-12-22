<template>
  <v-container>
    <v-bottom-navigation
    color="teal"
    grow
    class="pt-2 w-100 mb-3"
    >
      <v-btn href="#informacion">
        <span>INFORMACION</span>

        <v-icon>mdi-information</v-icon>
      </v-btn>

      <v-btn href="#servicios">
        <span>SERVICIOS</span>

        <v-icon>mdi-room-service</v-icon>
      </v-btn>

      <v-btn href="#comentarios">
        <span>COMENTARIOS</span>

        <v-icon>mdi-comment</v-icon>
      </v-btn>
    </v-bottom-navigation>
    <v-carousel
    cycle
    hide-delimiter-background
    show-arrows-on-hover>
      <v-carousel-item
        v-for="(item,i) in items"
        :key="i"
        :src="item.src"
        reverse-transition="fade-transition"
        transition="fade-transition"
      ></v-carousel-item>
    </v-carousel>
    <v-container id="informacion" class="pb-0 px-0">
      <v-card class="mx-auto d-flex"
        max-width="100%" elevation="18">
        <v-img
        height="400px"
        width="500px"        
        src="https://www.openmet.com/wp-content/uploads/2017/04/clima-laboral.jpeg"
        ></v-img>
        <div class="ma-3" >
          <v-card-title class="text-h5 font-weight-black">NUESTRAS SOLUCIONES</v-card-title>
          <v-card-text class="text-h6 text-justify">
            Somos una empresa de consultoría en talento humano dedicada al mejoramiento continuo del clima laboral de las empresas. A través de diferentes estudios se ha evidenciado que invertir en el bienestar de nuestros empleados no es un gasto sino una ganancia reflejada no solo en la productividad de la empresa sino también en el bienestar físico y emocional de nuestros empleados, mejorando así el clima laboral y el sentido de pertenencia por sus puestos. 
          </v-card-text>
        </div>
      </v-card>
    </v-container>
    <v-container id="servicios" class="pt-0 px-0">
      <v-row>
        <v-col
        v-for="(servicio, index) of servicios"        
        :key="index"
        :class="servicio.estado==1? true: 'd-none'"
        cols ="12"
        md="6"
        sm="6">
          <v-card
            class="mx-auto"
            max-width="100%"
          >
            <v-card-title>
              {{servicio.codigo}}
            </v-card-title>

            <v-img
              :src="servicio.imagen"
              alt="Imagenes"
              height="200px"
            ></v-img>

            <v-card-title>
              {{servicio.nombre}}
            </v-card-title>

            <v-card-subtitle>
              {{servicio.categoria.nombre}}
            </v-card-subtitle>

            <v-card-actions>
              <v-btn
                :to="'/articulos'"
                color="accent"
                text
              >
                Ver más
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
    <v-container id="comentarios" class="d-flex justify-space-between pt-0 px-0">
      <v-card
      v-for="(comentario, index) in comentarios"
      :key="index"
      class="elevation-16 mx-auto"
      width="300"
    >

    <v-img
      :src="comentario.imagen"
      max-height="150px"
      alt="Imagenes"
    ></v-img>
    <v-card-title class="headline">
      {{comentario.nombre}}
    </v-card-title>
    <v-card-text>
      {{comentario.comentario}}
    </v-card-text>
  </v-card>
    </v-container>
  </v-container>
</template>

<script>
  import axios from "axios";
  export default {
    name: "HelloWorld",
    data() {
      return {
        servicios: {},
        items: [
          {
            src: 'https://info.corponet.com.mx/hubfs/Imported_Blog_Media/importancia_de_la_gestion_de_talento_humano-2.jpg',
          },
          {
            src: 'https://www.ceupe.com/images/easyblog_articles/69/b2ap3_large_Ilustracion-publicacion-sofi.jpg',
          },
          {
            src: 'https://gerens.pe/blog/wp-content/uploads/2016/06/talento-humano.jpg',
          },
          {
            src: 'https://s3.amazonaws.com/images.pymas.com.co/_800xAUTO_crop_center-center/clima-laboral-en-una-empresa.jpg',
          },
        ],
        comentarios: [
          {
            nombre: "iEnglish",
            comentario: "La empresa realmente ofrece una solución bastante buena, hasta hace unos años teníamos bastantes problemas entre los empleados ya que ellos manifestaban estar cansados y gracias a las herramientas brindadas por ustedes hemos bajado la deserción en un 22% además de tener un mejor entorno laboral, realmente se notan los resultados",
            imagen: "https://image.freepik.com/vector-gratis/perfil-avatar-hombre-icono-redondo_24640-14044.jpg",
          },
          {
            nombre: "Reficaña",
            comentario: "Recomendamos sus servicios especialmente el de auditoria ya que nos permitió escuchar a nuestros empleados e identificar sus necesidades  y pudimos llegar a un acuerdo en nuestros beneficios laborales.",
            imagen: "https://www.flaticon.es/svg/static/icons/svg/3408/3408472.svg",
          },
          {
            nombre: "Cementos Pargos",
            comentario: "Hasta hace unos meses no podíamos conocer las necesidades de nuestros empleados, siendo una empresa que a pesar de pagar un buen sueldo nuestros empleados no duraban mucho tiempo con nosotros, definitivamente la clave de todo está en conocer las necesidades y ustedes ofrecen esa posibilidad en una web simple y fácil de manejar",
            imagen: "https://www.flaticon.es/svg/static/icons/svg/3565/3565209.svg",
          },
        ]
      };
    },
    mounted() {
      axios
        .get("https://cryptic-tor-85862.herokuapp.com/api/articulo/list")
        .then((response) => {
          (this.servicios = response.data)
          // console.log(this.servicios);
        });
    },
  };
</script>
