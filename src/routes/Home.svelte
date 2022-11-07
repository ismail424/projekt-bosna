<script>
    import Status from "../components/Status.svelte";
</script>
<div class="container">
    <div class="backbround"></div>
    <div class="content">
        <h1>Projekat Bosna 🇧🇦</h1>
        <p class="info">
            Mi, Bemuf Göteborg, zajedno sa Islamic Reliefom i Islamskom zajednicom Bosnjaka Göteborg pokrenuli smo projekat u Bosni i Hercegovini čiji je cilj renoviranje sirotišta ”JU DOM-porodica” u Zenici. 
        </p>
        <Status />
        <a href="/info">Više informacije </a>
        <div class="sponsor">
            <img src="/imgs/bemuf_lgo.png" alt="Sponsor">
            <img src="/imgs/islamic_relief.jpg" alt="Sponsor">
            <img src="/imgs/izgbg.jpeg" alt="Sponsor">
        </div>
    </div>
  </div>


<style lang="scss">


    .container {
        width: 100%;
        height: 100%;
        
        display: grid;
        grid-template-columns: 1fr 6fr 1fr;
        grid-template-rows: 0.4fr auto 1fr;
        grid-template-areas: 
        '. . .'
        '. content .'
        '. . .'; 
        justify-content: center;
        align-items: center;
    }

    .backbround {
        width: 100vw;
        height: 110vh;
        
        grid-area: 1/1/5/5;
        background: linear-gradient(0deg, rgba(0, 0, 0, 0.60), rgba(0, 0, 0, 0.70)), url("/imgs/bosna-background.webp");

        background-position: center;
        background-repeat: no-repeat;
        background-size: cover;
    }

    .content {
        height: 100%;
        
        grid-area: content;
        display: flex;
        flex-direction: column;
        gap: 1rem;
        
        color: white;
        background-color: #2a2a2a;
        padding: 50px 30px;
        border-radius: 16px;
        
        h1{
            font-size: 4rem;
        }
        p{
            font-size: 1.3rem;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            font-weight: 500;
        }
        a{
            font-size: 1.3rem;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            font-weight: 700;
            background-color: #3b88c3;
            border-radius: 0.3rem;
            text-decoration: none;
            text-align: center;
            padding:  0.5rem 1rem;
            color: white;
            &:hover{
                opacity: 0.8;
            }

        }
     
        .sponsor{
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 2rem;

            img{
                width: 100px;
                height: 100px;
                border-radius: 50%;
            }
        }

    }
    @media only screen and (max-width: 600px) {
        .content {
            padding: 40px 30px;
            height: auto;
            h1{
                font-size: 14vw;
            }
            p{
                font-size: 4vw;
            }
            .sponsor img {
                width: 15vw;
                height: 15vw;
            }
        }

    }

            

 </style>
 