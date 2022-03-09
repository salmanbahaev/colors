<script>
  import { onMount } from 'svelte';

	let colors;

	let agree = {
		Id: false,
		Name: false,
		Year: false,
		Color: false,
		Value: false
	}
	
	function setAgree(){
    setTimeout(() => localStorage.setItem('agree', JSON.stringify(agree)), 1);
	}

  onMount(async () => {
    setTimeout(() => localStorage.setItem('agree', JSON.stringify(agree)), 1);
    agree = JSON.parse(localStorage.getItem('agree'))
    if(agree.Id && agree.Name && agree.Year && agree.Color && agree.Value === true){
      resetColors()
    }
  });

  onMount(async () => {
    let response = await fetch('https://reqres.in/api/unknown?per_page=12')
		let content = await response.text()
    colors = JSON.parse(content)
  });
	
	async function resetColors() {
		let response = await fetch('https://reqres.in/api/unknown?per_page=12')
		let content = await response.text()
		colors = JSON.parse(content)
		agree.Id = agree.Name = agree.Year = agree.Color = agree.Value = true;
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
        <button on:click={resetColors} class:srcImg={agree.Id && agree.Name && agree.Year && agree.Color && agree.Value} class="title__btn"></button>
      </div>
			{#if colors}
			<div class="colors">
        <div class:hide={agree.Id !== true} class="colors__id">
          <label on:click={setAgree}><input id="id" type="checkbox" bind:checked={agree.Id}>ID</label>
        </div>
        <div class:hide={agree.Name !== true} class="colors__name">
          <label on:click={setAgree}><input id="name" type="checkbox" bind:checked={agree.Name}>NAME</label>
        </div>
        <div class:hide={agree.Year !== true} class="colors__year">
          <label on:click={setAgree}><input id="year" type="checkbox" bind:checked={agree.Year}>YEAR</label>
        </div>
        <div class:hide={agree.Color !== true} class="colors__color">
          <label on:click={setAgree}><input id="color" type="checkbox" bind:checked={agree.Color}>COLOR</label>
        </div>
        <div class:hide={agree.Value !== true} class="colors__value">
          <label on:click={setAgree}><input id="value" type="checkbox" bind:checked={agree.Value}>VALUE</label>
        </div>
      </div>
			{/if}
      
      <div class="colors__list" class:hide={(agree.Id || agree.Name || agree.Year || agree.Color || agree.Value) !== true}>
				{#if colors} 
				{#each colors.data as color}
					<div class="colors__item">
						<div class:hide={agree.Id !== true} class="colors__id">{color.id}</div>
						<div class:hide={agree.Name !== true} class="colors__name">{capitalize(color.name)}</div>
						<div class:hide={agree.Year !== true} class="colors__year">{color.year}</div>
						<div class:hide={agree.Color !== true} class="colors__color">
							<div class="colors__color-i" style="background-color: {color.color}"></div>
							<div>{color.color}</div>
						</div>
						<div class:hide={agree.Value !== true} class="colors__value">{color.pantone_value}</div>
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
  background-image: url("/img/reset-on.svg");
  background-repeat: no-repeat;
  background-position: center;
	background-color: #fff;
	border: none;
}
.title__btn:hover {
	cursor: pointer;
}

.srcImg {
	background-image: url("/img/reset-off.svg");
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

.colors input, .colors__item input {
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

.colors__id, .colors__item__id {
  min-width: 30px;
}

.colors__name, .colors__item__name {
  min-width: 170px;
}

.colors__year, .colors__item__year {
  min-width: 62px;
}

.colors__color, .colors__item__color {
  min-width: 115px;
}

.colors__value, .colors__item__value {
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