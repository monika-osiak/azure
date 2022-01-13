<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
    import Doughnut from "svelte-chartjs/src/Doughnut.svelte"

    let dataFetch = {"message": "", "tweets": "", "positive_percent": "", "negative_percent": "", "neutral_percent": "", "total_count": ""};
    let dataChart;
    let optionsChart = {
        legend: {
                position: "bottom",
                align: "center",
                labels: {
                    padding: 30,
                    fontSize: 20,
                    boxWidth: 50,
                }
            }
    }
	function onSubmit(e) {
    const formData = new FormData(e.target);
    const res = fetch("http://localhost:5001/", {
        method: "POST",
        body: formData
    })
    .then(response => response.json())
    .then(data2 => {
        for (let key in data2) {
            dataFetch[key] = data2[key];
        }
        console.log(dataFetch);
        dataChart = {
            labels: ['Negatywny','Pozytywny','Neutralny'],
            datasets: [
                {
                    data: [dataFetch["negative_percent"], dataFetch["positive_percent"], dataFetch["neutral_percent"]],
                    backgroundColor: [
                        'rgba(245, 0, 19, 0.8)',
                        'rgba(0, 245, 19, 0.8)',
                        'rgba(128, 128, 128, 0.8)'
                    ]
                }]
        };
    });
    }

</script>

<svelte:head>
	<title>Analiza sentymentu tweetów</title>
    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
</svelte:head>

<section>
    <div class="stream">
        <a href="/">
            <div class="stream-box">
                <h1>Analiza sentymentu tweetów</h1>
            </div>
        </a>

        <div class="stream-box">
            <form on:submit|preventDefault={onSubmit} method="post" action="/">
                <ul class="form-ul">
                    <li>
                        <label for="hashtag" id="hashtag">tag do analizy #</label>
                        <input type="text" name="hashtag" id="hashtag" required>
                    </li>
                    <li>
                        <label for="until">do dnia</label>
                        <input type="date" name="until" id="until" required>
                    </li>
                    <li>
                        <input type="submit" class="submit-button" value="Szukaj">
                    </li>
                </ul>
            </form>
        </div>

        {#if dataFetch["message"] == "" }
        {:else}
            <div class="stream-box error-box">
                { dataFetch["message"] }
            </div>
        {/if}

        {#if dataFetch["tweets"] == "" }
        {:else}
            <div class="stream-box">
                <h3>
                    Przeanalizowano { dataFetch["total_count"] } tweetów.
                </h3>
            </div>
            <!-- <div class="stream-box">
                <div class="chart-wrapper">
                    <Doughnut {dataChart} {optionsChart}/>
                     <canvas id="sentimentChart" bind:this={chartCanvas}></canvas>
                </div>
            </div> -->
            <div class="stream-box">
                <h3>
                    Oto przykładowe z nich:
                </h3>
            </div>
            {#each dataFetch["tweets"] as tweet }
                <div class="stream-box">
                    <div class="stream-box-date">
                        Opublikowano: { tweet.created_at }
                    </div>
                    <div class="stream-box-link">
                        <a href="{ 'https://twitter.com/' + tweet.author + '/status/' + tweet.id }">{ 'https://twitter.com/' + tweet.author + '/status/' + tweet.id }</a>
                    </div>
                    <div class="stream-box-content">
                        { tweet.content }
                    </div>
                    <div class="stream-box-sentiment">
                        Ocena: { tweet.sentiment }
                    </div>
                </div>
            {/each}
        {/if}

    </div>

</section>

<style>
	a {
    text-decoration: none;
}
body {
    font-family: 'Roboto', sans-serif;
    font-size: 1.5rem;
    background-color: #FBFBF2;
}
form {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
}
input {
    border: none;
    padding: 0;
    vertical-align: middle;
    height: 1.5rem
}
input[type='submit'] {
    background-color: #E6BEAE;
    font-family: 'Roboto', sans-serif;
}

h1 {
    text-align: center;
}

h3 {
    text-align: center;
}

h5 {
    text-align: center;
}

.stream {
    width: 80%;
    display: flex;
    flex-direction: column;
    margin: auto;
}
.stream-box {
    background-color: #EEE4E1;
    color: #21221B;
    border: 1px solid #E6BEAE;
    padding: 1rem;
    border-radius: 0.5rem;
    margin: 0.2rem;
}
.stream-box-date {
    font-size: 0.95rem;
}

.stream-box-link {
    font-size: 1.2rem;
}

.stream-box-link a {
    color:#25bef5;
}

.stream-box-link a:hover {
    color:#81d8f7;
}

.stream-box-content {
    margin: 0.5rem 0;
}
.stream-box-sentiment {
    text-align: end;
    font-size: 0.95rem;
    font-weight: bold;
}
.chart-wrapper {
    padding: 3%;
    position: relative; 
    margin: auto;
    width: 50%;
}

.mr-2 {
    margin-right: 2rem;
}

.error-box {
    background-color: #830707ab !important;
}

.form-ul li {
    align-items: center;
    list-style: none;
    display: inline;
    margin-right: 2rem;
}

.submit-button {
    text-align: center;
    align-items: center;
    width: 160px;
    height: 30px;
    margin: 5px;
    border-radius: 10px;
}

.submit-button:hover {
    background-color: #8f0d1e9d;
    color: white;
}
</style>
