<template>
  <div class="box">
    <img v-bind:src="`https://craterapi.com/${image}`" class="lazy" alt="" />
    <div class="columns pt-3">
      <div class="column">
        <p><b>Carrier Name:</b></p>
      </div>
      <div class="column">
        <p>
          <b>
            {{ name }}
          </b>
        </p>
      </div>
    </div>
    <div class="columns">
      <div class="column">
        <p><b>Expiry Type:</b></p>
      </div>
      <div class="column">
        <p>
          <b>
            {{ expiry }}
          </b>
        </p>
      </div>
    </div>
    <div class="columns">
      <div class="column">
        <p><b>Plan Size:</b></p>
      </div>
      <div class="column">
        <p>
          <b>{{ size }} {{ unit }}</b>
        </p>
      </div>
    </div>
    <div class="columns">
      <div class="column">
        <p><b>Country Code:</b></p>
      </div>
      <div class="column">
        <p>
          <b>{{ country }}</b>
        </p>
      </div>
    </div>

    <nav>
      <a :href="link">View Terms and Conditions</a>
    </nav>
  </div>
</template>

<script>
export default {
  props: ["id", "expiry", "size", "link", "unit", "name", "image", "country"],
};
</script>

<style scoped>
h3 {
  font-size: 1.25rem;
  margin: 0.5rem 0;
}

p {
  font-size: 11px;
}

a {
  text-decoration: none;
  color: #ce5c00;
}

a:hover,
a:active {
  color: #c89300;
}
</style>
