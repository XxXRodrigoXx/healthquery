<script>
    import QuizMassa from "$components/QuizMassa.svelte";
    import GabaritoMassa from "$components/GabaritoMassa.svelte";
	import { goto } from "$app/navigation";
    import { loadFromSessionStorage, saveToSessionStorage } from '../../sessionStorage';

    let perguntas = [
        {
            PerguntaEmQuestao: "Qual é a quantidade recomendada de exercício físico por semana?",
            R1: "75 minutos de atividade intensa",
            R2: "150 minutos de atividade moderada",
            R3: "Ambas as opções a e b",
            R4: "Nenhuma das opções acima",
            correta: 3
        },
        {
            PerguntaEmQuestao: "Quanto tempo um adulto deveria dormir?",
            R1: "10 horas por dia",
            R2: "7 até 9 horas em média",
            R3: "Apenas uma soneca de 30 minutos já basta",
            R4: "Entre 5 a 6 horas",
            correta: 2
        },
        {
            PerguntaEmQuestao: "Um adulto deve consumir 2L de água por dia em média?",
            R1: "Sim, pois é o necessário para ser saudável",
            R2: "Não, pois é muita água pro nosso organismo",
            R3: "Depende, a quantidade de água que consumimos varia com o nosso peso e a perda de água que temos no dia (suor, urina e outros)",
            R4: "Depende, pois devemos beber somente quando estamos com sede para hidratarmos",
            correta: 3
        },
        {
            PerguntaEmQuestao: "Sobre o sono, como deveria ser o ambiente quando dormir em relação a luz?",
            R1: "Com o mínimo de luz possível, que ajuda a produção de melatonina (hormônio do sono) que melhora a manutenção do nosso corpo enquanto dormimos",
            R2: "Com um pouco de luz (abajur ou velas) porque o nosso corpo se sente mais seguro quando podemos enxergar um pouco no escuro",
            R3: "Com luzes azuis gerados por aparelhos tecnológicos para cansar nossa vista e dormirmos melhor",
            R4: "Tanto faz, quando fechamos os olhos toda luz é evitada e o sono será o mesmo",
            correta: 1
        },
        {
            PerguntaEmQuestao: "Segundo a OMS (Organização Mundial da Saúde) considerando uma dieta de 2000 calorias, quantas gramas de açúcar é recomendado a um adulto consumir diariamente?",
            R1: "50g",
            R2: "200g",
            R3: "1000g",
            R4: "1200g",
            correta: 1
        },
        {
            PerguntaEmQuestao: "Sobre o sono, qual é a posição recomendada e o motivo?",
            R1: "De barriga pra cima, pois melhora a circulação do sangue e previne o ronco no sono",
            R2: "De barriga pra baixo, pois aumenta a tensão do corpo e relaxa melhor os músculos",
            R3: "Do lado esquerdo, pois não faz pressão sobre os órgãos internos e mantém a coluna reta",
            R4: "De barriga pra cima, pois é a melhor posição pra dormir e relaxar todos os músculos do corpo",
            correta: 3
        },
        {
            PerguntaEmQuestao: "Quais os benefícios do exercício físico no nosso dia?",
            R1: "Apenas para ganharmos músculos e ficarmos fortes",
            R2: "Reduz o estresse e sintomas de ansiedade, melhora a qualidade do sono, entre outros benefícios no corpo",
            R3: "Nenhum, é apenas obrigação do dia para levantarmos e fazermos as coisas do dia",
            R4: "No há benefícios, apenas nos desgasta e é apenas um hobby para algumas pessoas",
            correta: 2
        },
        {
            PerguntaEmQuestao: "Quais são bons momentos para lavar as mãos?",
            R1: "Antes de comer",
            R2: "Após o uso do banheiro",
            R3: "Na volta para casa",
            R4: "Todos estão corretos",
            correta: 4
        },
        {
            PerguntaEmQuestao: "Qual destes alimentos é rico em proteínas?",
            R1: "Maçã",
            R2: "Pão branco",
            R3: "Frango grelhado",
            R4: "Batata frita",
            correta: 3
        },
        {
            PerguntaEmQuestao: "Qual destes é um benefício da meditação?",
            R1: "Aumenta a força muscular",
            R2: "Melhora a saúde cardiovascular",
            R3: "Reduz o estresse",
            R4: "Aumenta a altura",
            correta: 3
        },
    ];

    let pergunta_id = 0;
    let acertou = 0;
    let respondeu = false;
    let acertouUltima = false;
    let respostaSelecionada = 0;

    /**
	 * @param {{ detail: number; }} e
	*/
    function handleAnswer(e) {
        respostaSelecionada = e.detail;
        const perguntaAtual = perguntas[pergunta_id];
        acertouUltima = respostaSelecionada === perguntaAtual.correta;
        if (acertouUltima) {
            acertou += 1;
        }
        respondeu = true;
    }

    function handleNextQuestion() {
        if (pergunta_id < perguntas.length - 1) {
            pergunta_id += 1;
            respondeu = false;
            respostaSelecionada = 0;
        } else {
            let texto = `Quiz concluído! Você acertou ${acertou} de ${perguntas.length} perguntas.`;
            sessionStorage.setItem('texto', texto);
            goto('/quiz/resultado');
            saveToSessionStorage("dinheiro", Number(loadFromSessionStorage("dinheiro") || 0) + acertou)
        }
    }
</script>

{#if !respondeu}
    <QuizMassa {pergunta_id} {perguntas} on:answer={handleAnswer} />
{:else}
    <GabaritoMassa {pergunta_id} {perguntas} {acertouUltima} {respostaSelecionada} on:next={handleNextQuestion}></GabaritoMassa>
{/if}