<script lang="ts">
  import { Machine, interpret } from "xstate";

  let trafficLightMachine = Machine({
    id: "lights",
    initial: "red",
    states: {
      red: {
        on: { GO: "green" },
      },
      green: {
        on: { WARN: "yellow" },
      },
      yellow: {
        on: { STOP: "red" },
      },
    },
  });

  let action;
  let lightState
  let service = interpret(trafficLightMachine).onTransition((obs) => {
    console.log(obs.value);
    lightState = obs.value
  });

  lightState = 'red'
  service.start();

  function handleSubmit() {
    if (action) {
      console.log("sending: " + action);
      service.send(action);
    }
  }
</script>

<style>
  .traffic-lights {
    max-width: 800px;
    margin: 0 auto;
  }
  h1 {
    text-align: center;
  }

  .light-box {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .light-box > * {
    margin: 1rem;
  }

  .bulb {
    width: 50px;
    height: 50px;
    border: 2px #444;
    border-radius: 50%;
    background: #666;
  }

  .yellow {
    background: rgb(238, 238, 101);
  }
  .red {
    background: rgb(216, 52, 52);
  }

  .green {
    background: rgb(57, 113, 57);
  }

  .form {
    text-align: center;
  }
</style>

<div class="traffic-lights">
  <h1>traffic ligns</h1>

  <div class="form">
    <select bind:value={action}>
      <option value="GO">GO</option>
      <option value="WARN">WARN</option>
      <option value="STOP">STOP</option>
    </select>
    <button on:click={handleSubmit}>Submit</button>
  </div>

  <div class="light-box">
    <div class="bulb" class:green={lightState == 'green'} />
    <div class="bulb" class:yellow={lightState == 'yellow'} />
    <div class="bulb" class:red={lightState == 'red'} />
  </div>
</div>
