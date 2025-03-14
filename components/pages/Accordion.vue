<template>
  <section class="Cont flex">
    <div class="basis-full sm:basis-11/12 md:basis-10/12">
      <div
        v-for="(item, index) in accordionItems"
        :key="index"
        :class="item.expanded ? 'Active' : ''"
      >
        <h3 class="Header" @click="directToggle(index)">
          {{ item.title }}
        </h3>
        <div class="Content">
          <p>{{ item.content }}</p>
          <i class="Base"></i>
        </div>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
export default {
  setup() {
    const accordionItems = ref([
      {
        title: "Intuitive UI Designs",
        content: `Enhance user experiences with user-friendly interfaces that engage and convert.`,
      },
      {
        title: "Dynamic Web Apps",
        content: `Deliver mobile-app-like experiences with fast navigation and seamless user interactions.`,
      },
      {
        title: "Robust APIs and AI integrations",
        content: `Take your applications to the next level with powerful APIs and AI tools.`,
      },
    ]);

    let intervalId;

    const toggleAccordion = (index) => {
      accordionItems.value.forEach((item, i) => {
        if (i === index) {
          item.expanded = !item.expanded;
        } else {
          item.expanded = false;
        }
      });
    };

    const expandAccordionAutomatically = () => {
      const currentIndex = accordionItems.value.findIndex(
        (item) => item.expanded
      );

      const nextIndex =
        currentIndex === accordionItems.value.length - 1 ? 0 : currentIndex + 1;

      toggleAccordion(nextIndex);
    };

    const directToggle = (index) => {
      clearInterval(intervalId);
      toggleAccordion(index);
      intervalId = setInterval(expandAccordionAutomatically, 5000);
    };

    onMounted(() => {
      accordionItems.value[0].expanded = true;
      intervalId = setInterval(expandAccordionAutomatically, 5000);
    });

    onUnmounted(() => {
      if (intervalId) {
        clearInterval(intervalId);
      }
    });

    return {
      accordionItems,
      toggleAccordion,
      directToggle,
    };
  },
};
</script>

<style lang="scss" scoped>
.Cont {
  // min-height: 350px;
  min-height: 270px;
  padding-left: 20px;
}
.Header {
  background-color: $sec-color-trans-2;
  background: linear-gradient(
    to right,
    rgb(39, 64, 96, 0.5),
    rgb(39, 64, 96, 0) 90%
  );
  color: $pri-color;
  padding: 10px;
  cursor: pointer;
  position: relative;
  margin: 0px;
}

.Content {
  position: relative;
  background-color: transparent;
  overflow: hidden;
  color: white;
  max-height: 0px;
  padding-right: 30px;
}

.Active .Content {
  max-height: 250px;
  transform: translateX(20px);
  transition: 1s cubic-bezier(0.4, -0.65, 0.265, 2);
}

.Base {
  position: absolute;
  bottom: 5px;
  left: 0;
  height: 3px;
  border-radius: 28px;
  background-color: $pri-color;
}

.Active .Base {
  width: 100%;
  animation: accord-border 2s cubic-bezier(0.42, 0, 0.58, 1);
}

@keyframes accord-border {
  0% {
    width: 0px;
  }
  100% {
    width: 100%;
  }
}
</style>
