<template>
  <div class="main-wrapper" @mousemove="updateMouse">
    
    <!-- Luz violeta del cursor -->
    <div 
      class="cursor-glow" 
      :style="{ left: mouseX + 'px', top: mouseY + 'px' }"
    ></div>

    <div class="container py-5 content-relative">
      
      <!-- Encabezado -->
      <header class="text-center mb-5 animate-fade-in">
        <div class="profile-container mb-4">
          <img src="./assets/img/pusheenchips2.png" alt="Foto de Perfil" class="profile-img">
        </div>
        <h1 class="display-4 fw-bold text-white mb-2">{{ nombre }}</h1>
        
        <!-- DESCRIPCIÓN CON EFECTO TYPING DINÁMICO -->
        <div class="description-container mx-auto">
          <p class="description-text">
            {{ textoAnimado }}<span class="cursor-typing">|</span>
          </p>
        </div>

        <div class="mt-4">
          <button @click="scrollToContact" class="btn btn-glass px-4">Contactar</button>
        </div>
      </header>

      <div class="row justify-content-center">
        
        <!-- Habilidades -->
        <div class="col-12 col-lg-8 mb-5">
          <div class="card card-glass glow-card">
            <div class="card-body p-4 text-center">
              <h3 class="h5 mb-4 text-white border-bottom border-secondary pb-2">Habilidades</h3>
              <div class="d-flex flex-wrap justify-content-center gap-4">
                <div v-for="skill in habilidades" :key="skill.name" class="icon-box">
                  <i :class="skill.icon" class="display-5 text-white"></i>
                  <p class="small mt-2 opacity-75">{{ skill.name }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Trayectoria Académica -->
        <div class="col-12 col-lg-8 mb-5">
          <div class="card card-glass glow-card">
            <div class="card-body p-4">
              <h3 class="h5 mb-4 text-white border-bottom border-secondary pb-2 text-center">Trayectoria Académica</h3>
              <div class="timeline">
                <div v-for="(curso, index) in cursos" :key="index" class="timeline-item">
                  <div class="timeline-dot"></div>
                  <div class="timeline-content">
                    <div class="d-flex justify-content-between align-items-center flex-wrap gap-2">
                      <div>
                        <h4 class="h6 mb-1 text-white">{{ curso.titulo }}</h4>
                        <span class="small opacity-75">{{ curso.plataforma }}</span>
                      </div>
                      <span :class="['badge-status', curso.estado === 'Completado' ? 'bg-success' : 'bg-info']">
                        {{ curso.estado }}
                      </span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Formulario de Contacto -->
        <div ref="contactSection" class="col-12 col-lg-8 mb-5">
          <div class="card card-glass glow-card p-4">
            <h3 class="text-center text-white mb-4">Contáctame</h3>
            <form @submit.prevent="handleSubmit">
              <div class="mb-3">
                <input type="text" class="form-control input-glass" placeholder="Nombre completo" required>
              </div>
              <div class="mb-3">
                <input type="email" class="form-control input-glass" placeholder="Correo electrónico" required>
              </div>
              <div class="mb-3">
                <textarea class="form-control input-glass" rows="4" placeholder="¿En qué puedo ayudarte?" required></textarea>
              </div>
              <button type="submit" class="btn btn-glass w-100 py-3 mt-2">Enviar Mensaje</button>
            </form>
          </div>
        </div>

        <!-- SECCIÓN DE REDES SOCIALES -->
        <div class="col-12 text-center mt-4">
          <div class="social-links d-flex justify-content-center gap-4">
            <a href="https://github.com/repos" target="_blank" class="social-btn github">
              <i class="devicon-github-original"></i>
            </a>
            <a href="https://www.linkedin.com/in/pedro-montalban-a491ba271/" target="_blank" class="social-btn linkedin">
              <i class="devicon-linkedin-plain"></i>
            </a>
          </div>
          <p class="mt-4 small opacity-50 text-white">© {{ new Date().getFullYear() }} - Pedro Montalban</p>
        </div>

      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const nombre = ref("Pedro Montalban")

const frases = [
  "Soy una persona apasionada por el desarrollo web y en la creación de soluciones digitales interactivas.",
  "Especialista en Vue.js.",
  "Diseño interactivo con CSS y JS.",
  "Busco optimizar la experiencia de los usuarios escribiendo código limpio y diseños responsivos."
]

const textoAnimado = ref("")
const mouseX = ref(0)
const mouseY = ref(0)

let fraseIndex = 0
let charIndex = 0
let estaBorrando = false

const manejarEscritura = () => {
  const fraseActual = frases[fraseIndex]
  if (estaBorrando) {
    textoAnimado.value = fraseActual.substring(0, charIndex - 1)
    charIndex--
  } else {
    textoAnimado.value = fraseActual.substring(0, charIndex + 1)
    charIndex++
  }

  let velocidad = estaBorrando ? 30 : 60
  if (!estaBorrando && charIndex === fraseActual.length) {
    velocidad = 2000 
    estaBorrando = true
  } else if (estaBorrando && charIndex === 0) {
    estaBorrando = false
    fraseIndex = (fraseIndex + 1) % frases.length
    velocidad = 500
  }
  setTimeout(manejarEscritura, velocidad)
}

onMounted(() => { manejarEscritura() })

const updateMouse = (e) => {
  mouseX.value = e.clientX
  mouseY.value = e.clientY
}

const contactSection = ref(null)
const scrollToContact = () => {
  contactSection.value?.scrollIntoView({ behavior: 'smooth' })
}

const habilidades = ref([
  { name: "HTML5", icon: "devicon-html5-plain" },
  { name: "CSS3", icon: "devicon-css3-plain" },
  { name: "Vue.js", icon: "devicon-vuejs-plain" },
  { name: "Node.js", icon: "devicon-nodejs-plain" },
  { name: "GitHub", icon: "devicon-github-original" },
  { name: "JavaScript", icon: "devicon-javascript-plain" }
])

const cursos = ref([
  { titulo: "Desarrollador de Aplicaciones Front-End Trainee", plataforma: "Kibernum Academy", estado: "En curso" },
  { titulo: "Arquitectura de Microservicios", plataforma: "Platzi", estado: "En curso" },
  { titulo: "UI/UX Design con Figma", plataforma: "Coursera", estado: "Completado" }
])

const handleSubmit = () => { alert("¡Gracias! Tu mensaje ha sido enviado.") }
</script>

<style scoped>
.main-wrapper {
  position: relative;
  min-height: 100vh;
  background: linear-gradient(135deg, #1e3a8a 0%, #6b21a8 100%);
  color: white;
  padding-bottom: 50px;
  overflow: hidden; 
}

.cursor-glow {
  position: fixed;
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, rgba(168, 85, 247, 0.15) 0%, rgba(168, 85, 247, 0) 70%);
  border-radius: 50%;
  pointer-events: none;
  transform: translate(-50%, -50%);
  z-index: 1;
}

.content-relative {
  position: relative;
  z-index: 2;
}

/* Typing & Desc */
.description-container { max-width: 750px; min-height: 90px; margin-top: 15px; }
.description-text {
  font-size: 1.2rem; line-height: 1.6; font-weight: 500;
  background: linear-gradient(to right, #ffffff, #a855f7, #ffffff);
  background-size: 200% auto; color: transparent;
  -webkit-background-clip: text; background-clip: text;
  animation: shine 4s linear infinite; display: inline;
}
.cursor-typing { color: #a855f7; font-weight: bold; animation: blink 0.8s infinite; margin-left: 4px; }
@keyframes blink { 50% { opacity: 0; } }
@keyframes shine { to { background-position: 200% center; } }

/* Cards & Glass */
.card-glass {
  max-width: 800px; margin: 0 auto;
  background: rgba(255, 255, 255, 0.07);
  backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.2); 
  border-radius: 24px; transition: all 0.4s ease;
}
.glow-card:hover {
  box-shadow: 0 0 25px rgba(168, 85, 247, 0.25);
  border: 1px solid rgba(255, 255, 255, 0.4);
  transform: translateY(-5px);
}

/* Redes Sociales con Efecto Glass */
.social-btn {
  font-size: 2.5rem;
  color: white;
  width: 70px;
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 50%;
  backdrop-filter: blur(10px);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  text-decoration: none;
}

.social-btn:hover {
  transform: scale(1.1) translateY(-10px);
  background: rgba(255, 255, 255, 0.15);
  color: white;
}

.github:hover { box-shadow: 0 0 20px rgba(255, 255, 255, 0.4); border-color: rgba(255, 255, 255, 0.5); }
.linkedin:hover { box-shadow: 0 0 20px rgba(0, 119, 181, 0.6); border-color: rgba(0, 119, 181, 0.5); }

/* Resto de estilos base */
.input-glass { background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(255, 255, 255, 0.15); color: white; border-radius: 12px; padding: 12px; }
.input-glass:focus { background: rgba(255, 255, 255, 0.1); border-color: #a855f7; box-shadow: 0 0 15px rgba(168, 85, 247, 0.2); }
.icon-box { transition: all 0.3s ease; width: 80px; }
.icon-box:hover { transform: scale(1.2); color: #a855f7; filter: drop-shadow(0 0 8px rgba(168, 85, 247, 0.6)); }
.profile-img { width: 150px; height: 150px; border-radius: 50%; border: 3px solid rgba(255, 255, 255, 0.2); box-shadow: 0 0 35px rgba(168, 85, 247, 0.4); }
.btn-glass { background: rgba(255, 255, 255, 0.1); border: 1px solid rgba(255, 255, 255, 0.2); color: white; border-radius: 50px; transition: 0.3s; }
.btn-glass:hover { background: rgba(168, 85, 247, 0.4); box-shadow: 0 0 15px rgba(168, 85, 247, 0.4); }
.timeline { max-width: 600px; margin: 0 auto; position: relative; padding-left: 30px; border-left: 2px solid rgba(255, 255, 255, 0.15); }
.timeline-item { position: relative; margin-bottom: 25px; padding-left: 15px; transition: transform 0.3s ease; }
.timeline-item:hover { transform: translateX(10px); }
.timeline-dot { position: absolute; left: -31px; top: 5px; width: 14px; height: 14px; background: #a855f7; border-radius: 50%; box-shadow: 0 0 15px #a855f7; }
.timeline-content { background: rgba(255, 255, 255, 0.05); padding: 12px 18px; border-radius: 15px; border: 1px solid rgba(255, 255, 255, 0.1); }
.badge-status { font-size: 0.7rem; padding: 4px 10px; border-radius: 20px; background: rgba(255, 255, 255, 0.1); }
</style>
