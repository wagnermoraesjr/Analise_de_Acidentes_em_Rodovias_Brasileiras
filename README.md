# Análise de Acidentes em Rodovias Brasileiras
Análise dos registros de ocorrência de acidente em rodovias brasileiras com dados de 2021 a 2023, para entendimento, insights e recomendações. 

![imagem_estrada](https://github.com/wagnermoraesjr/Analise_de_Acidentes_em_Rodovias_Brasileiras/blob/main/foto-da-estrada-de-asfalto.jpg)

## **Introdução**

As rodovias brasileiras desempenham um papel fundamental em nossa conectividade e mobilidade, permitindo o transporte de pessoas e mercadorias por todo o país. No entanto, o uso intensivo dessas vias também traz consigo desafios significativos relacionados à segurança rodoviária. Acidentes de trânsito são eventos trágicos que impactam a vida de milhares de brasileiros anualmente, causando perdas humanas, danos materiais e onerosos custos sociais e econômicos.
<br><br>
A análise de dados de acidentes em rodovias brasileiras assume uma importância fundamental na busca por soluções eficazes para mitigar esses eventos.
<br><br>

## **Objetivo**

Através das combinações detalhadas de informações disponibilizadas sobre os acidentes nas rodovias brasileiras, aplicações de técnicas de análise estatística juntamente com dados geoespaciais, faremos essa análise para responder perguntas cruciais, como por exemplo:
- Quais regiões registram os maiores números de ocorrências de acidente?
- Quais os dias da semana e período do dia com mais registros?
- Existe alguma relação entre as condições meteorológicas ou caracteristicas da pista e os acidentes?
- Qual o tipo de acidente, causa e a gravidade mais comuns dos acidentes?
- Quais BRs possuem o maior número de acidentes?
- Qual a média de mortos, feridos graves, feridos leves e ilesos?
- Existe uma relação espacial ou temporal relevante para esses incidentes?
- Como a presença de radares e outras medidas de segurança influenciam nas taxas de acidentes?

A partir das descobertas feitas nessa análise poderemos gerar insights e recomendações importantes às autoridades, não somente para prevenção e segurança rodoviária, mas também para o direcionamento mais eficaz dos recursos disponíveis.
<br><br>

## **Conjunto de Dados**

A **Polícia Rodoviária Federal – PRF** atende cerca de 70 mil quilômetros de rodovias federais e está distribuída em todo o território nacional, combatendo a criminalidade, prestando auxílio ao cidadão, fiscalizando, autuando e atendendo acidentes.
<br><br>
O registro de acidentes é realizado através do sistema BAT, que coleta informações referentes aos envolvidos (identificação, estado físico, se era passageiro, condutor, etc.), ao local, aos veículos, à dinâmica do acidente, etc. Os dados disponíveis têm origem nos sistemas BR-Brasil e BAT. O sistema BR-Brasil foi utilizado em nível nacional entre 2007 e 2016. O sistema BAT é utilizado desde 2017.
<br><br>
Para essa análise iremos utilizar os Dados Abertos sobre os **Registros de Ocorrência de Acidente (Agrupados por ocorrência) dos últimos três anos (2021-2023)** extraídos do **[Portal de Dados Abertos da PRF](https://www.gov.br/prf/pt-br/acesso-a-informacao/dados-abertos/dados-abertos-da-prf)**. Dados Abertos são dados institucionais, disponibilizados em formato legível por máquina e sem restrição de licenças, patentes ou mecanismos de controle, que qualquer pessoa pode livremente usá-los, reutilizá-los e redistribuí-los.
<br><br>
Para enriquecer ainda mais essa análise, iremos utilizar também os **Dados dos Radares** disponibilizados pela **[Agência Nacional de Transportes Terrestres](https://dados.gov.br/dados/conjuntos-dados/radar)**, registrados no Sistema de Informação de Rodovias da ANTT.
<br><br>
**Dicionário de Dados - Acidentes (Agrupados por ocorrência):**
<br>
**`id`**: Variável com valores numéricos, representando o identificador do acidente.
<br>
**`data_inversa`**: Data da ocorrência no formato dd/mm/aaaa.
<br>
**`dia_semana`**: Dia da semana da ocorrência. Ex.: Segunda, Terça, etc.
<br>
**`horario`**: Horário da ocorrência no formato hh:mm:ss.
<br>
**`uf`**: Unidade da Federação. Ex.: MG, PE, DF, etc.
<br>
**`br`**: Variável com valores numéricos representando o identificador da BR do acidente.
<br>
**`km`**: Identificação do quilômetro onde ocorreu o acidente, com valor mínimo de 0,1 km e com a casa decimal separada por ponto.
<br>
**`municipio`**: Nome do município de ocorrência do acidente.
<br>
**`causa_acidente`**: Identificação da causa principal do acidente. Ex.: Falta de atenção, Velocidade incompatível, etc. Neste conjunto de dados são excluídos os acidentes com a variável causa principal igual a "Não".
<br>
**`tipo_acidente`**: Identificação do tipo de acidente. Ex.: Colisão frontal, Saída de pista, etc. Neste conjunto de dados são excluídos os tipos de acidentes com ordem maior ou igual a dois. A ordem do acidente demonstra a sequência cronológica dos tipos presentes na mesma ocorrência.
<br>
**`classificação_acidente`**: Classificação quanto à gravidade do acidente. Ex.: Sem Vítimas, Com Vítimas Feridas, Com Vítimas Fatais e Ignorado.
<br>
**`fase_dia`**: Fase do dia no momento do acidente. Ex.: Amanhecer, Pleno dia, etc.
<br>
**`sentido_via`**: Sentido da via considerando o ponto de colisão (crescente e decrescente).
<br>
**`condição_meteorologica`**: Condição meteorológica no momento do acidente. Ex.: Céu claro, chuva, vento, etc.
<br>
**`tipo_pista`**: Tipo da pista considerando a quantidade de faixas (dupla, simples ou múltipla).
<br>
**`tracado_via`**: Descrição do traçado da via (reta, curva ou cruzamento).
<br>
**`uso_solo`**: Descrição sobre as características do local do acidente (urbano = sim ou rural = não).
<br>
**`pessoas`**: Total de pessoas envolvidas na ocorrência.
<br>
**`mortos`**: Total de pessoas mortas envolvidas na ocorrência.
<br>
**`feridos_leves`**: Total de pessoas com ferimentos leves envolvidas na ocorrência.
<br>
**`feridos_graves`**: Total de pessoas com ferimentos graves envolvidas na ocorrência.
<br>
**`ilesos`**: Total de pessoas ilesas envolvidas na ocorrência.
<br>
**`ignorados`**: Total de pessoas envolvidas na ocorrência e que não se soube o estado físico.
<br>
**`feridos`**: Total de pessoas feridas envolvidas na ocorrência (é a soma dos feridos leves com os graves).
<br>
**`veiculos`**: Total de veículos envolvidos na ocorrência.
<br>
**`latitude`**: Latitude do local do acidente em formato geodésico decimal.
<br>
**`longitude`**: Longitude do local do acidente em formato geodésico decimal.
<br>
**`regional`**: Informação não encontrada.
<br>
**`delegacia`**: Informação não encontrada.
<br>
**`uop`**: Informação não encontrada.
<br><br>
**Dicionário de Dados - Radares:**
<br>
**`concessionaria`**: Nome da Concessionária que prestou as informações.
<br>
**`ano_do_pnv_snv`**: Ano que representa as informações no Plano Nacional de Viação e Sistema Nacional de Viação. Ex.: 2017.
<br>
**`tipo_de_radar`**: Tipo de radar (Redutor ou Controlador).
<br>
**`rodovia`**: Rodovia responsável pela concessionária. Ex.: BR-116.
<br>
**`uf`**: Sigla do Estado da rodovia. Ex.: SP.
<br>
**`km_m`**: Representação do quilômetro mais a metragem. Ex.: 317,940.
<br>
**`Município`**: Município da localização do radar.
<br>
**`tipo_de_pista`**: Tipo da pista que está localizada a sinalização (principal ou marginal).
<br>
**`sentido`**: Representação da ordem crescente ou decrescente ou crescente/decrescente.
<br>
**`situacao`**: Situação atual (ativo ou inativo).
<br>
**`data_da_inativacao`**: Data de registro, caso a situação seja dada como inativo.
<br>
**`latitude`**: Representação de Coordenadas. Ex.: -22,490967.
<br>
**`longitude`**: Representação de Coordenadas. Ex.: -44,561228.
<br>
**`velocidade_leve`**: Velocidade da Rodovia veículos leves.
<br>
**`velocidade_pesado`**: Velocidade da Rodovia veículos pesados.

<br><br>
**Abrir o Notebook no Google Colab:** [Análise de Acidentes em Rodovias Brasileiras](https://colab.research.google.com/drive/1Y2-SL2BUXCNNvTuGnd7vxZugkYVELcQV?usp=drive_link)

**Baixar o Notebook:** [Análise de Acidentes em Rodovias Brasileiras](https://github.com/wagnermoraesjr/Analise_de_Acidentes_em_Rodovias_Brasileiras/raw/main/Projeto_Analise_dos_Acidentes_em_Rodovias_Brasileiras_github.ipynb)
