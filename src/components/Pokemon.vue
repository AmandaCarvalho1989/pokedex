<template>
  <div id="pokemon">
    <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currImage" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ name | upper }}</p>
            <p class="subtitle is-6">
              {{ pokemon.type }}
              <img src="" alt="" />
            </p>
          </div>
        </div>

        <div class="content">
          <button
            class="button is-fullwidth is-info is-outlined"
            @click="changeSprite"
            id="action"
          >
            Mudar sprite
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    name: String,
    url: String,
  },
  data() {
    return {
      isFront: true,
      currImage: "",
      typeImage: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },

  created: function () {
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;

      this.currImage = this.pokemon.front;
    });
  },
  methods: {
    changeSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currImage = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currImage = this.pokemon.front;
      }
    },
  },
  filters: {
    upper: function (value) {
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
};
</script>

<style>
#pokemon {
  margin-top: 1.5rem;
}

.card-image figure img {
  width: 50%;
  height: 50%;
  transition: 0.4 all;
}

#action {
  color: #2468b1;
  border-color: #2468b1;
}
#action:hover {
  background-color: #2468b1;
  color : white;
}
</style>