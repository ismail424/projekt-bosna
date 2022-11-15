<script lang="ts">
      import { t } from '../i18n';
</script>

<div class="container">
    <div class="content">
        <h1>{$t('info.title')}</h1>
            {#each $t('info.text') as text}
                <p>{text}</p>
            {/each}
        <div class="images">
            <img src="/imgs/info/image00066.jpeg" alt="Dom-porodica Zenica" >
            <img src="/imgs/info/image00070.jpeg" alt="Dom-porodica Zenica">
            <img src="/imgs/info/image00071.jpeg" alt="Dom-porodica Zenica" >
            <img src="/imgs/info/image00080.jpeg" alt="Dom-porodica Zenica">

        </div>
        <br><br>
    </div>
</div>


<style lang="scss">

    .container{
        margin: 4vh 6vw;
        
        h1{
            text-align: center;
            font-size: 40px;
        }
        .content{
            display: flex;
            flex-direction: column;
            justify-content: center;
            gap: 20px;
            p{
                line-height: 1.5;
            }
            .images{
                display: flex;
                flex-direction: row;
                justify-content: space-between;
                gap: 10px;
                width: 100%;
                max-height: 400px;
                img{
                    width: 100%;
                }
            }
        }

    }
    @media only screen and (max-width: 600px){
        .container{
            .content{
                .images{
                    flex-direction: column;
                    max-width: 100%;
                    max-height: 100%;
                }
            }
        }
    }

    @media only screen and (min-width: 600px) and (max-width: 1200px){
        .container{
            .content{
                .images{
                    flex-direction: column;
                    max-width: 100%;
                    max-height: 100%;
                }
            }
        }
    }
</style>