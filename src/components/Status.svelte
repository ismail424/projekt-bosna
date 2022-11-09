<script>
    import ProgressBar from 'progressbar.js';
    import PocketBase from 'pocketbase';
    import { onMount } from 'svelte';
    const client = new PocketBase('https://pb.sacic.dev');

    let goal = 250_000;
    let raised = 0;
    
    let progressbar = null;
    let progressbar_text_span = document.createElement('span');
    progressbar_text_span.innerText = ` kr `;
    progressbar_text_span.className = 'goal-text';

    const get_total_raised = async () => {
        try {
            const total_sum = await client.records.getFullList('projekt_bosna', 200 );
            raised = 0;
            await total_sum.forEach((item) => {
                raised += item.belopp;
            });
            return raised;
        } catch (e) {
            console.log(e);
            return 0;
        }
    }

    onMount(async () => {
        raised = await get_total_raised(); 
        console.log(raised);  
        let raised_percent = (raised / goal);

        progressbar = new ProgressBar.Line('#progress', {
            color: '#eee',
            trailColor: '#eee',
            trailWidth: 1,
            duration: 1400,
            easing: 'bounce',
            strokeWidth: 4,
            from: { color: '#b0ff45', a: 0 },
            to: { color: '#64813d', a: 1 },
            step: (state, circle) => {
                circle.path.setAttribute('stroke', state.color);
            }
        });
        progressbar.animate(raised_percent);
        progressbar.setText(` 💵 ${raised.toLocaleString('fr-FR')} `);
        progressbar.text.appendChild(progressbar_text_span);
    });


    $: progress = raised / goal * 100;

</script>
<div class="whole_progress">
    <div class="whole_text">
        Koliko smo dosada skupili para 💰
    </div>
    <p>
        <span>0 kr</span>
        <span>Cilj {goal.toLocaleString('fr-FR')} kr</span>
    </p>
    <div id="progress"></div>
</div>


<style lang="scss">

    .whole_progress {
        width: 100%;
        height: 20vh;

        p {
            display: flex;
            justify-content: space-between;
            margin: 0;
            padding: 10px;
            font-size: 1rem;
            align-items: center;
        }
        .whole_text{
            background-color: #3c3c3c;
            padding: 0.4rem;
            border-radius: 0.2rem;
        }
    }

    :global(.progressbar-text) {
        font-size: 1.5rem;
        margin-top: 3rem !important;
        font-weight: 800;
    }
    :global(.goal-text) {
        font-size: 1rem;
        font-weight: 400;
    }

    @media only screen and (max-width: 600px) {
        :global(.progressbar-text) {
            position: unset !important;
            top: unset !important;
            left: unset !important;
            transform: unset !important;

            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: row;
            margin-top: 1rem !important;

            :global(span){
                margin: 0 0.5rem;
                margin-top: 0.4rem ;
            }
        }
    }
</style>