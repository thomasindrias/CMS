<template>
  <div :class="{'animated fadeIn': lazyLoad}">
    <section v-scroll-reveal.reset="{ delay: 250 }" class="section container-box">
      <div class="columns is-centered is-multiline">
        <div class="column is-4 svg-box-profile ">
          <img class="profile " src="../assets/svg/profile.svg" alt="">
        </div>
        <div class="column is-full text-box title is-size-1 is-size-3-mobile has-text-centered">
          <span>Hi, my name is Thomas! </span>
        </div>
      </div>
    </section>
    
    <section v-scroll-reveal.reset="{ delay: 250, duration: 1000 }" class="section container-box-big bg-gradient">
      <div class="columns ">
        <div class="column text-box title is-size-1 is-size-3-mobile has-text-centered-mobile has-text-white-ter">
          <span>I develop web and native apps.</span>
        </div>
        <div class="column svg-box is-three-fifths">
          <img class="svg-2" src="../assets/svg/dev.svg" alt="">
        </div>
      </div>
    </section>

    <section v-scroll-reveal.reset="{ delay: 250 }" class="section container-box">
      <div class="columns reverse-row-order ">
        <div class="column text-box title is-size-2 is-size-3-mobile has-text-right has-text-centered-mobile">
          <span>Also a Master Science Student in Media Technology and Engineering.</span>
        </div>
        <div class="column svg-box is-three-fifths">
          <img class="svg-size-1" src="../assets/svg/education.svg" alt="">
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import anime from 'animejs'
import client from '~/plugins/contentful'

export default {
  components: {},
  data: function() {
    return {
      lazyLoad: false
    }
  },
  asyncData() {
    return (
      client
        .getEntries({
          content_type: 'post',
          order: '-sys.createdAt'
        })
        .then(entries => {
          // eslint-disable-next-line no-console
          // console.log(entries.items)

          return {
            posts: entries.items
          }
        })
        // eslint-disable-next-line no-console
        .catch(e => console.log(e))
    )
  },
  mounted: function() {
    this.lazyLoad = true

    const profile = anime.timeline({
      duration: 300,
      easing: 'easeInOutQuad'
    })

    profile
      .add(
        {
          targets: '.profile',
          opacity: '1',
          scale: [
            { value: '0%', duration: 500 },
            { value: '100%', duration: 500 }
          ]
        },
        '+=200'
      )
      .add({
        targets: '.text-box span',
        opacity: '1'
      })
  },
  head() {
    return {
      title: 'About',
      meta: [
        {
          hid: 'title',
          property: 'og:title',
          content: 'About'
        },
        {
          hid: 'image',
          property: 'og:image',
          content: '/profile.png'
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
$black: #4a4a4a;

.container-box {
  padding: 10% 20%;
  margin: 10px 0;
  position: relative;
}

.container-box-big {
  padding: 30% 10%;
  position: relative;
}

.svg-box {
  margin-top: 2vh;
}

.svg-size-1 {
  height: 90%;
}

.svg-box-profile {
  width: 270px;
  height: 270px;
}

svg-box img {
  width: 0;
  height: 0;
}

.text-box span {
  opacity: 0;
}

.bg-gradient {
  background-image: linear-gradient(
    to left bottom,
    #cb4ec6,
    #ab55cb,
    #895acd,
    #635dca,
    #325ec4
  );
}
.reverse-row-order {
  flex-direction: row-reverse;
}
</style>
