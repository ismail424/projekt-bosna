<div class="container">
    <div class="backbround"></div>
    <div class="content">
        <h1>Projekt Bosna</h1>
    </div>
  </div>


<style lang="scss">


    .container {
        width: 100%;
        height: 100%;
        
        display: grid;
        grid-template-columns: 1fr 6fr 1fr;
        grid-template-rows: 1fr 6fr 1fr;
        grid-template-areas: 
        '. . .'
        '. content .'
        '. . .'; 
        justify-content: center;
        align-items: center;
    }

    .backbround {
        width: 100vw;
        height: 100vh;
        
        grid-area: 1/1/5/5;
        background: linear-gradient(0deg, rgba(0, 0, 0, 0.60), rgba(0, 0, 0, 0.70)), url("/imgs/bosna-background.webp");

        background-position: center;
        background-repeat: no-repeat;
        background-size: cover;
    }

    .content {
        height: 200px;
        position: relative;
        
        grid-area: content;
        display: flex;
        
        justify-content: center;
        align-items: center;
        color: white;
        h1{
            font-size: 4rem;
        }
    }

            

 </style>
 