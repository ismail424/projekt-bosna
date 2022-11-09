<script>
    import PocketBase from 'pocketbase';
    import { onMount } from 'svelte';
    import ProgressBar from 'progressbar.js';

    const client = new PocketBase('https://pb.sacic.dev');

    let top_donations = [];
    let data = [];
    let error = null;

    let raised = 0;
    let goal = 250_000;
    var progressbar = null;

    const raised_percent = ( ) => {
        return raised / goal;
    }

    const raised_percent_text = ( ) => {
        return `${(raised_percent() * 100).toFixed(2)}%`;
    }
    
    client.realtime.subscribe('projekt_bosna', async (event) => {
        raised = await get_total_raised();
        progressbar.animate(raised_percent());
        progressbar.setText(raised_percent_text());
        await refresh_donations();
      
    });

    const get_total_raised = async () => {
        try {
            const total_sum = await client.records.getFullList('projekt_bosna', 200 , {
                sort: '-created',
            });
            raised = 0;
            await total_sum.forEach((item) => {
                raised += item.belopp;
            });
            return raised;
        } catch (e) {
            return 0;
        }
    }
    const get_top_donations = async () => {
        try {
            const top_results = await client.records.getFullList('projekt_bosna', 3, {
                sort: '-belopp',
            });
            return top_results.slice(0, 3);
        } catch (e) {
            console.log(e);
            return [];
        }
    }

    const get_latest_donations = async () => {
        try {
            const latest_results = await client.records.getList('projekt_bosna', 1, 6, {
                sort: '-created',
            });
            data = latest_results.items;      
            return data
        } catch (e) {
            console.log(e);
            return [];
        }
    }

    const refresh_donations = async () => {

        raised = await get_total_raised();

        data = await get_latest_donations();

        // Get the top 5 donations
        top_donations = await get_top_donations();
    }


    onMount(async () => {
        try {
            await refresh_donations();

            progressbar = new ProgressBar.Circle('#progress', {
                color: '#fff',
                trailColor: '#eee',
                trailWidth: 5,
                duration: 1400,
                easing: 'bounce',
                strokeWidth: 10,
                fill: '#769551',
                from: { color: '#fff', a: 0 },
                to: { color: '#eeba11', a: 1 },
                step: (state, circle) => {
                    circle.path.setAttribute('stroke', state.color);
                }
            });

            progressbar.animate(raised_percent());
            progressbar.setText(raised_percent_text());
            
        } catch (e) {
            console.error(e);
            error = e;
        }
    });





    function timeSince(date) {
  
        date = new Date(date);
        date.setHours(date.getHours() + 1);

        let seconds = Math.floor((new Date() - date) / 1000);
        let interval = seconds / 31536000;

        interval = seconds / 86400;
        if (interval > 1) {
        return Math.floor(interval) + " days";
        }
        interval = seconds / 3600;
        if (interval > 1) {
        return Math.floor(interval) + "h";
        }
        interval = seconds / 60;
        if (interval > 1) {
        return Math.floor(interval) + "min";
        }
        return Math.floor(seconds) + "sec";
    }

</script>

<div class="realtime_display">   
    <h2>Cilj {goal.toLocaleString('fr-FR')} kr </h2> 
    <div id="progress"></div>
    <h3>Skupili smo {raised.toLocaleString('fr-FR')} kr 💰</h3>

    <div class="donation-section">
        {#if error}
            <span style="color: red;">{error.message}</span>
        {/if}

        {#each top_donations as donation}
            <card class="top_donations">
                <p id="{donation.id}">
                        {#if top_donations.indexOf(donation) == 0}
                        🥇
                        {:else if top_donations.indexOf(donation) == 1}
                        🥈
                        {:else if top_donations.indexOf(donation) == 2}
                        🥉
                        {/if}

                 {donation.belopp.toLocaleString('fr-FR')} kr
                 </p>
            </card>
        {/each}
        <br>    
        {#if data}
            {#each data as record}
                <card>
                    <p id="{record.id}"> + {record.belopp.toLocaleString('fr-FR')} kr 
                        <span>
                        {
                            timeSince(new Date(record.created)) == "0sec" ? "sada" : timeSince(new Date(record.created)) + " ago"
                        }
                        </span>
                        
                    </p>
                </card>
            {/each}
        {/if}
    </div>
</div>


<style lang="scss">
    .realtime_display{
        width: 100vw;
        height: 100vh;
        padding: 3rem 0;
        background-color: #64813d;
        color: white;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap:1rem;

        .donation-section{
            display: flex;
            flex-direction: column;
            gap: 1rem;
            width: 100%;
            align-items: center;
            .top_donations{
                p {
                    color: #4a4949;
                }
                &:first-of-type{
                    background-color: gold ;
                }
                &:nth-of-type(2){
                    background-color: silver;

                }
                &:nth-of-type(3){
                    background-color : #cd7f32;
                }
            }

            card{
                width: 60% ;
                background-color:#fff;
                border-radius: 10px;
                padding: 0.6rem 1rem;
                p {
                    font-size: 1.2rem;
                    font-weight: 700;
                    text-align: center;
                    color: #3f423f;
                }
                span{
                    margin-left: 0.5rem;
                    font-size: 1rem;
                    opacity: 0.6;
                    font-weight: 400;
                    color: #3f423f;
                }
            }
        }
    
        
        #progress {
            width: 30vw;
            height: 30vw;
            position: relative;
            margin-bottom: 1rem;
            :global(.progressbar-text) {
                font-size: 3vw !important;
                font-weight: 700;
                color: white;
            }
        }


        @media only screen and (max-width: 600px) {
            #progress {
                width: 20vh;
                height: 20vh;
                :global(.progressbar-text) {
                    font-size: 6vw !important;
                    font-weight: 700;
                    color: white;
                    margin-top: -7rem !important;
                }
            }
        }
        @media only screen and (max-width: 350px) {
            #progress {
                :global(.progressbar-text) {margin-top: -5rem !important;}
            }
        }
    
    }
</style>