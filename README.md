# 🚀 Automação de Relatório Semanal de Mídias Sociais e Notícias

Este projeto demonstra uma solução de automação de ponta a ponta (iPaaS/RPA) desenvolvida no n8n. O objetivo é eliminar o trabalho manual de coleta, cálculo e distribuição de relatórios de desempenho e tendências.

# 🎯 O Desafio

O processo manual de monitoramento de KPIs de crescimento de mídias sociais (Instagram, TikTok, YouTube) e a curadoria de notícias relevantes do setor (RSS Feeds) consumia tempo semanal significativo.

A meta era: Consolidar todos esses dados em um único relatório, executar cálculos de crescimento e arquivar/enviar o resultado de forma totalmente autônoma.

# ⚙️ Solução Técnica (Visão Geral do Workflow)

O fluxo é agendado para rodar semanalmente (usando o Schedule Trigger) e segue a seguinte lógica:
Etapa	Nó Utilizado	Habilidade Demonstrada
1. Coleta de KPIs	Get row(s) in sheet	Integração com Google Sheets para leitura de dados estruturados (Seguidores Atuais vs. Semana Passada).
2. Coleta de Notícias	RSS Read	Integração com RSS Feeds (Google News) para curadoria de notícias de tecnologia/IA.
3. Combinação de Dados	Merge (Append)	Transformação de Dados: Combina as 303 notícias e os 3 KPIs em um único stream de dados.
4. Cálculos de KPIs	Code in JavaScript	Manipulação de Dados: Utiliza JavaScript para iterar sobre os KPIs e calcular a Diferença Absoluta e o Crescimento Percentual (Crescimento_Percentual, Diferenca).
5. Consolidação e Formatação	Edit Fields (Set)	Data Transformation: Cria a string final Relatorio_Semanal (em formato texto, com quebras de linha \n) combinando todos os KPIs calculados e os títulos/links das notícias.
6. Log e Auditoria	Append row in sheet	Salva a string completa do relatório na planilha de histórico.
7. Entrega	Send a message (Gmail)	Envia o relatório final para as partes interessadas.

# 💻 Código JavaScript (Cálculo de Crescimento)

    for (const item of items) {
      // Converte strings para Number para garantir o cálculo:
      const atuais = Number(item.json.Seguidores_Atuais);
      const passados = Number(item.json.Seguidores_Semana_Passada);
    
    const diferenca = atuais - passados;
    const crescimento_percentual = (diferenca / passados) * 100;
  
    // Adiciona os novos KPIs ao item:
    item.json.Diferenca = diferenca;
    item.json.Crescimento_Percentual = crescimento_percentual.toFixed(2);
    }
  
    return items;

# ✨ Resultados e Impacto

    100% Autônomo: Automação completa do ciclo de vida do relatório (coleta, processamento, log, entrega).

    Acuracidade: Cálculo automático de KPIs de crescimento (eliminando erros manuais).

    Log: Histórico permanente de todos os relatórios enviados, garantindo compliance e auditoria.

# 📥 Como Usar (Para Teste)

    Baixe o arquivo workflow/Automação de Relatório Semanal de Mídia Social.json.

    Importe o arquivo .json para sua instância n8n.

    Conecte as credenciais necessárias (Google Sheets, Gmail).
