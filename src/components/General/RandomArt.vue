<template>
  <span/>
</template>

<script>
export default {
  props: {
    seed: { default: 2 },
    text: { default: false },
    type: { default: 'profile' },
  },
  mounted () {
    this.$el.appendChild(this.box)
  },
  watch: {
    box (box, prevBox) {
      this.$el.replaceChild(box, prevBox)
    },
  },
  computed: {
    initials () {
      if (!this.text) return
      let parts = this.text.split(' ').map(s => s[0])
      if (parts.length > 2) {
        parts = [parts[0], parts[parts.length - 1]]
      }
      return parts.join('').toUpperCase()
    },
    box () {
      function pseudoRandom (seed) {
        let random = Math.sin(seed) * 10000
        random -= Math.floor(random)
        return random
      }

      let seed = pseudoRandom(this.seed)

      function getRandomRange (min, max, add = 1000) {
        return Math.floor(pseudoRandom(seed * add) * (max - min) + min)
      }

      const svgns = 'http://www.w3.org/2000/svg'
      const box = document.createElementNS(svgns, 'svg')

      let rows = 3
      let columns = 3
      let ratio = 1.0

      if (this.type === 'banner') {
        rows = 7
        columns = 7
        ratio = 1.3
      }

      const size = 100
      const blockSize = Math.floor(size / rows * 1.5)
      const blockSizeHeight = Math.floor(blockSize / ratio)
      const rotate = blockSize * rows / 2

      box.setAttribute('xmlns:xlink', 'http://www.w3.org/1999/xlink')
      box.setAttribute('viewBox', '0 0 100 100')
      box.setAttribute('class', 'box')

      const g = document.createElementNS(svgns, 'g')
      g.setAttribute(
        'transform',
        `translate(${-(rows * blockSize - size) / 2} ${-((rows * blockSize - size) / 2)}) ` +
        `rotate(${90 * seed} ${rotate} ${rotate})`,
      )

      for (let i = 0; i < columns; i++) {
        // noprotect
        for (let j = 0; j < rows; j++) {
          let rect = document.createElementNS(svgns, 'rect')
          rect.setAttribute('width', blockSize)
          rect.setAttribute('height', blockSizeHeight)
          rect.setAttribute('fill', 'rgba(' +
            getRandomRange(100, 255, (i + 1) * (j + 1) * 1) + ',' +
            getRandomRange(100, 255, (i + 1) * (j + 1) * 2) + ',' +
            getRandomRange(100, 255, (i + 1) * (j + 1) * 3) + ',1)',
          )
          rect.setAttribute('x', i * blockSize)
          rect.setAttribute('y', j * blockSizeHeight)

          g.appendChild(rect)
        }
      }
      box.appendChild(g)

      let overlay = document.createElementNS(svgns, 'rect')
      overlay.setAttribute('width', size)
      overlay.setAttribute('height', size)
      overlay.setAttribute('fill', 'rgba(' +
        getRandomRange(100, 255, 1) + ',' +
        getRandomRange(100, 255, 2) + ',' +
        getRandomRange(100, 255, 3) + ',0.5)',
      )
      overlay.setAttribute('x', 0)
      overlay.setAttribute('y', 0)
      box.appendChild(overlay)

      let textOverlay = document.createElementNS(svgns, 'text')
      textOverlay.setAttribute('width', size)

      if (this.text && this.type === 'banner') {
        let text = document.createTextNode(this.text)
        textOverlay.setAttribute('fill', 'rgba(' +
          getRandomRange(210, 250, 1) + ',' +
          getRandomRange(210, 250, 2) + ',' +
          getRandomRange(210, 250, 3) + ',1)',
        )
        textOverlay.setAttribute('font-size', 5)
        textOverlay.setAttribute('font-weight', 'bold')
        textOverlay.setAttribute('text-anchor', 'start')
        textOverlay.setAttribute('x', 3)
        textOverlay.setAttribute('y', 4.5)
        textOverlay.appendChild(text)
      }

      if (this.text && this.type === 'profile') {
        let text = document.createTextNode(this.initials)
        textOverlay.setAttribute('fill', 'rgba(255,255,255,1)')
        textOverlay.setAttribute('font-size', 50)
        textOverlay.setAttribute('text-anchor', 'middle')
        textOverlay.setAttribute('x', 50)
        textOverlay.setAttribute('y', 66)
        textOverlay.appendChild(text)
      }

      box.appendChild(textOverlay)
      return box
    },
  },
}
</script>

<style scoped lang='stylus'>
</style>
