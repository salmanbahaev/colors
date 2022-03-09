<script>
  import { onMount } from 'svelte';

	var colors = 0;
  var agree = {}

  function agreeDefault() {
    agree.id = true;
    agree.name = true;
    agree.year = true;
    agree.color = true;
    agree.value = true;
  }

  agreeDefault()

  console.log('agree', agree)
	
	function setAgree(){
    setTimeout(() => localStorage.setItem('agree', JSON.stringify(agree)), 1);
	}

  onMount(async () => {
    console.log('onMount', agree)
    setAgree()
    console.log('SETAGREE', agree)
    agree = JSON.parse(localStorage.getItem('agree'));
    
    console.log('AGREEEEEE', agree)
    if(agree.id && agree.name && agree.year && agree.color && agree.value){
      console.log('dsadadadsa', agree)
    }
  });

  onMount(async () => {
    const response = await fetch('https://reqres.in/api/unknown?per_page=12')
		const content = await response.text()
    colors = JSON.parse(content)
  });
	
	async function resetColors() {
		const response = await fetch('https://reqres.in/api/unknown?per_page=12')
		const content = await response.text()
		colors = JSON.parse(content)
		agreeDefault()
    setAgree()
	}

  const capitalize = (s) => {
    if (typeof s !== 'string') return ''
    return s.charAt(0).toUpperCase() + s.slice(1)
  }

</script>

<main>
	<div class="wrapper">
    <div class="container">
      <div class="title">
        <div class="title__text">Pantone colors</div>
        <button on:click={resetColors} class="title__btn {(agree.id && agree.name && agree.year && agree.color && agree.value) === true ? 'srcImg' : ''}"></button>
      </div>
			{#if colors}
			<div class="colors">
        <div class:hide={agree.id !== true} class="colors__id">
          <label on:click={setAgree}><input id="id" type="checkbox" bind:checked={agree.id}>ID</label>
        </div>
        <div class:hide={agree.name !== true} class="colors__name">
          <label on:click={setAgree}><input id="name" type="checkbox" bind:checked={agree.name}>NAME</label>
        </div>
        <div class:hide={agree.year !== true} class="colors__year">
          <label on:click={setAgree}><input id="year" type="checkbox" bind:checked={agree.year}>YEAR</label>
        </div>
        <div class:hide={agree.color !== true} class="colors__color">
          <label on:click={setAgree}><input id="color" type="checkbox" bind:checked={agree.color}>COLOR</label>
        </div>
        <div class:hide={agree.value !== true} class="colors__value">
          <label on:click={setAgree}><input id="value" type="checkbox" bind:checked={agree.value}>VALUE</label>
        </div>
      </div>
			{/if}
      
      <div class="colors__list {(agree.id || agree.name || agree.year || agree.color || agree.value) === true ? '' : 'hide'}">
				{#if colors} 
				{#each colors.data as color}
					<div class="colors__item">
						<div class:hide={agree.id !== true} class="colors__id">{color.id}</div>
						<div class:hide={agree.name !== true} class="colors__name">{capitalize(color.name)}</div>
						<div class:hide={agree.year !== true} class="colors__year">{color.year}</div>
						<div class:hide={agree.color !== true} class="colors__color">
							<div class="colors__color-i" style="background-color: {color.color}"></div>
							<div>{color.color}</div>
						</div>
						<div class:hide={agree.value !== true} class="colors__value">{color.pantone_value}</div>
					</div>
				{/each}
				{/if}
				
      </div>
    </div>
  </div>
</main>

<style>

:global(body) {
  font-family: 'Segoe UI', sans-serif;
  margin: 0;
  padding: 0;
}

* {
  margin: 0;
}

.wrapper {
  background-color: #ECF0F3;
}

.container {
  font-family: 'Segoe UI', sans-serif;
  margin: 0 auto;
  width: 640px;
  background-color: white;
}

.title {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: justify;
      -ms-flex-pack: justify;
          justify-content: space-between;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  height: 64px;
  padding-left: 24px;
  padding-right: 24px;
  font-weight: bold;
  font-size: 24px;
  line-height: 32px;
}

.title__btn {
  width: 62px;
  height: 22px;
  background-image: url("../img/reset-on.svg");
  background-repeat: no-repeat;
  background-position: center;
	background-color: #fff;
	border: none;
}
.title__btn:hover {
	cursor: pointer;
}

.srcImg {
	background-image: url("../img/reset-off.svg");
}
.srcImg:hover {
	cursor: default;
}
.colors, .colors__item {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-pack: distribute;
      justify-content: space-around;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  font-size: 12px;
  line-height: 16px;
  border-top: 1px solid rgba(0, 0, 0, 0.08);
}

.colors > div, .colors__item > div {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 8px;
}

.colors input {
  margin-right: 5px;
}

.colors div:first-child, .colors__item div:first-child {
  border: none;
}

.colors__item {
	font-size: 16px;
	line-height: 21px;
	height: 53px;
}

.colors__id {
  min-width: 30px;
}

.colors__name {
  min-width: 170px;
}

.colors__year {
  min-width: 62px;
}

.colors__color {
  min-width: 115px;
}

.colors__value {
  min-width: 162px;
}

.colors__item .colors__id {
  text-align: right;
}
.colors__item .colors__year {
	text-align: center;
}
.colors__item .colors__color {
	display: flex;
	align-items: center;
}
.colors__item .colors__color-i {
	width: 16px;
	height: 16px;
	border-radius: 2px;
	margin-right: 4px;
}

.colors .hide,
.colors__list .hide,
.colors__list.hide {
	display: none;
}

</style>