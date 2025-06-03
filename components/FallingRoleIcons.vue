<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
const { isMobile, isTablet } = useDevice();

// === Icon Imports ===
import WarriorIcon      from '~/assets/icons/warrior.svg'
import DarkknightIcon   from '~/assets/icons/darkknight.svg'
import GunbreakerIcon   from '~/assets/icons/gunbreaker.svg'
import PaladinIcon      from '~/assets/icons/paladin.svg'
import WhiteMageIcon    from '~/assets/icons/whitemage.svg'
import AstrologianIcon  from '~/assets/icons/astrologian.svg'
import ScholarIcon      from '~/assets/icons/scholar.svg'
import SageIcon         from '~/assets/icons/sage.svg'
import BardIcon         from '~/assets/icons/bard.svg'
import MachinistIcon    from '~/assets/icons/machinist.svg'
import DancerIcon       from '~/assets/icons/dancer.svg'
import NinjaIcon        from '~/assets/icons/ninja.svg'
import SamuraiIcon      from '~/assets/icons/samurai.svg'
import DragoonIcon      from '~/assets/icons/dragoon.svg'
import MonkIcon         from '~/assets/icons/monk.svg'
import ReaperIcon       from '~/assets/icons/reaper.svg'
import BlackmageIcon    from '~/assets/icons/blackmage.svg'
import SummonerIcon     from '~/assets/icons/summoner.svg'
import RedmageIcon      from '~/assets/icons/redmage.svg'

// === Responsive Settings ===
const ICON_COUNT    = isMobile ? 10  : isTablet ? 20 : 30
const MIN_SIZE      = isMobile ? 56 : 126
const MAX_SIZE      = isMobile ? 90 : 196
const MIN_SPEED     = 0.01
const MAX_SPEED     = 0.03
const MIN_OPACITY   = 0.15
const MAX_OPACITY   = 0.33
const MIN_ANGULAR   = 0.07
const MAX_ANGULAR   = 0.1
const ICON_COLOR    = null

const ICONS = [
  { component: WarriorIcon },
  { component: DarkknightIcon },
  { component: GunbreakerIcon },
  { component: PaladinIcon },
  { component: WhiteMageIcon },
  { component: AstrologianIcon },
  { component: ScholarIcon },
  { component: SageIcon },
  { component: BardIcon },
  { component: MachinistIcon },
  { component: DancerIcon },
  { component: NinjaIcon },
  { component: SamuraiIcon },
  { component: DragoonIcon },
  { component: MonkIcon },
  { component: ReaperIcon },
  { component: BlackmageIcon },
  { component: SummonerIcon },
  { component: RedmageIcon }
]

const icons = ref([])

for (let i = 0; i < ICON_COUNT; i++) {
  const iconDef = ICONS[Math.floor(Math.random() * ICONS.length)]
  const size = MIN_SIZE + Math.random() * (MAX_SIZE - MIN_SIZE)
  icons.value.push({
    id: i,
    component: iconDef.component,
    x: Math.random() * 100,
    y: Math.random() * 100,
    vx: (Math.random() < 0.5 ? -1 : 1) * (MIN_SPEED + Math.random() * (MAX_SPEED - MIN_SPEED)),
    vy: (Math.random() < 0.5 ? -1 : 1) * (MIN_SPEED + Math.random() * (MAX_SPEED - MIN_SPEED)),
    size,
    opacity: MIN_OPACITY + Math.random() * (MAX_OPACITY - MIN_OPACITY),
    color: ICON_COLOR,
    angle: Math.random() * 360,
    vAngle: (Math.random() < 0.5 ? -1 : 1) * (MIN_ANGULAR + Math.random() * (MAX_ANGULAR - MIN_ANGULAR))
  })
}

let animationFrame

const animate = () => {
  for (const icon of icons.value) {
    icon.x += icon.vx
    icon.y += icon.vy
    icon.angle = (icon.angle + icon.vAngle) % 360

    // Bounce off edges
    const vw = icon.size / window.innerWidth * 100
    const vh = icon.size / window.innerHeight * 100
    if (icon.x < 0)      { icon.x = 0;      icon.vx *= -1; icon.vAngle *= -1 }
    if (icon.x > 100-vw) { icon.x = 100-vw; icon.vx *= -1; icon.vAngle *= -1 }
    if (icon.y < 0)      { icon.y = 0;      icon.vy *= -1; icon.vAngle *= -1 }
    if (icon.y > 100-vh) { icon.y = 100-vh; icon.vy *= -1; icon.vAngle *= -1 }
  }
  animationFrame = requestAnimationFrame(animate)
}

onMounted(() => {
  animationFrame = requestAnimationFrame(animate)
})
onBeforeUnmount(() => {
  cancelAnimationFrame(animationFrame)
})
</script>

<template>
  <div class="fixed inset-0 pointer-events-none -z-10">
    <component
      v-for="icon in icons"
      :key="icon.id"
      :is="icon.component"
      :style="{
        position: 'absolute',
        left: icon.x + 'vw',
        top: icon.y + 'vh',
        width: icon.size + 'px',
        height: icon.size + 'px',
        opacity: icon.opacity,
        filter: 'drop-shadow(0 2px 8px rgba(0,0,0,0.13))',
        color: icon.color,
        transition: 'none',
        transform: `rotate(${icon.angle}deg)`
      }"
    />
  </div>
</template>
