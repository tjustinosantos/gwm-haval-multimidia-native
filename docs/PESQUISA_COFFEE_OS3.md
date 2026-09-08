# Pesquisa pública: Haval H6 e Coffee OS 3

Consulta em **08/09/2026**. Alvo provisório: central de 14,6 polegadas do H6 brasileiro renovado, com Coffee OS 3. A versão exata do carro e do firmware ainda não foi informada. Este documento complementa a [análise do repositório e do Impulse](COMPATIBILIDADE_H6_2026.md).

## Resultado e alcance

Nas fontes consultadas, **não foi localizado um dump identificado, firmware público com procedência verificável ou procedimento reproduzível que estabeleça compatibilidade com o H6 brasileiro Coffee OS 3**. Há projetos úteis para a geração anterior e relatos sobre outras centrais, mas falta uma correspondência verificável com a unidade brasileira. Isso não demonstra impossibilidade técnica nem ausência de soluções privadas.

A pesquisa incluiu GitHub e web em português, inglês, russo e chinês, documentação da GWM, páginas dos próprios projetos e relatos de usuários. As buscas de repositórios incluíram `haval infotainment`, `gwm android`, `haval in:name` e `dargo in:name`. Nas duas últimas, foram examinados os primeiros 100 resultados ordenados por atualização; não houve varredura integral do GitHub. Datas recentes de publicação não foram tratadas como evidência do ano do veículo ou da geração da central.

## O que a documentação da GWM permite concluir

- O [comunicado brasileiro do novo H6](https://www.gwmmotors.com.br/pt/media-center/news/2025/gwm-apresenta-o-novo-haval-h6-adaptado-ao-gosto-dos-motoristas-brasileiros) confirma Coffee OS 3, tela de 14,6 polegadas e barra persistente personalizável. Ele não informa os contratos Android necessários para portar o Impulse.
- A [apresentação de resultados de 2025](https://res.gwm.com.cn/2025/08/29/1849740_131_2025%E5%B9%B4%E4%B8%AD%E6%9C%9F%E4%B8%9A%E7%BB%A9.pdf#page=46), página impressa 45/46, relaciona H6, Xiaolong, Xiaolong MAX, Raptor, Dargo de segunda geração e WEY Latte ao grupo V3.5/Coffee OS 3. É uma pista para procurar referências de plataformas relacionadas, não uma lista de firmwares intercambiáveis nem a identificação da central brasileira.
- O [relatório oficial de 2023](https://res.gwm.com.cn/2023/08/30/1834443_131_e101.pdf#page=31), página impressa 29, já relacionava **V3.5 a Coffee OS 2**, instalado no WEY Blue Mountain DHT-PHEV e Dargo de segunda geração. Portanto, o nome V3.5 sozinho não identifica Coffee OS 3. Também não permite deduzir a versão Android ou o processador da unidade brasileira.

## Projetos que podem ajudar numa adaptação

| Projeto / fonte primária | Evidência consultada | Utilidade e limite |
|---|---|---|
| [Impulse / HavalShisuku](https://github.com/bobaoapae/haval-app-tool-multimidia) | Inicialização por Telnet/Shizuku e integrações Beantechs/Autolink no código; participantes da [issue 46](https://github.com/bobaoapae/haval-app-tool-multimidia/issues/46) relatam falta de suporte ao Coffee OS 3. | Principal base de implementação já examinada. O acesso, os contratos e os displays precisam ser comparados com a nova central. Os comentários da issue são relatos da comunidade. |
| [Haval Radio](https://github.com/leandrosavn/haval-radio#readme) | README declara Android 9 como alvo, Shizuku e serviço `com.beantechs.intelligentvehiclecontrol`, com propriedades `sys.radio.*`. | Exemplo menor para estudar a camada de rádio. Não contém, na documentação examinada, validação do Coffee OS 3 brasileiro. |
| [AA-Cluster](https://github.com/maicongoe/AA-Cluster#readme) | README depende de Shizuku com UID 0 e Impulse; o patch estático é restrito à build OEM `bean07021019` e ao hash do AndroidAutoService esperado. | Mostra que integração profunda exige correspondência de firmware. Esse patch não serve como pacote genérico para testar na central nova. |
| [Haval Engine Reverse](https://github.com/rocamoras/haval-engine-reverse) | [Documento de WeatherService](https://github.com/rocamoras/haval-engine-reverse/blob/2ac25d790a12d7c905bd717e33d48088c849275f/docs/weatherservice-reverse.md) descreve reconstrução do contrato AIDL de um componente Beantechs versão `1.0.0.2023.11.06` a partir do launcher. | Referência de método e contratos antigos. Não identifica uma coleta de Coffee OS 3. A data da análise em 2026 não transforma o componente em firmware da nova geração. |
| [GWM Engineering Menu](https://github.com/bndrv/gwm-eng-menu#readme) | Autor declara teste no **H9 Gen2 2024, central Desay SV**, usando diagnóstico CAN. | Pista para investigar a família correta da central. Não comprova suporte ao H6; o procedimento altera o estado do menu, portanto não equivale a uma coleta passiva. |

O próprio [VoltDash](https://voltdash.com.br/) declara no FAQ compatibilidade com H6 HEV/PHEV/GT 2023–2025 e exclusão dos modelos com Coffee OS 3. Isso confirma a limitação declarada desse produto; não prova uma limitação universal do sistema.

## Pistas de Dargo e de outros mercados

**Dargo/Coffee OS 3/Android 11:** o resultado indexado de uma [discussão no 4PDA](https://4pda.to/forum/index.php?showtopic=1110681&st=4600) menciona Coffee OS 3, Android 11 e Dargo 2026. O conteúdo integral retornou HTTP 403. Sem contexto completo, identificação da central e artefato verificável, esse trecho permanece uma pista de baixa confiança. **Android 11 não foi confirmado para o H6 brasileiro.**

O autor do [DargoSplit apresenta instalação via ADB e recursos sem root](https://rutube.ru/video/2509012f418b463d568bb72d14654b81/). A descrição consultada não identifica Coffee OS 3, o ano da central ou uma build compatível com o Brasil. A versão 3 do aplicativo também não indica a geração do sistema do carro.

Um [relato no Reddit sobre instalação de apps no H6 2026](https://www.reddit.com/r/HavalTipsAndTricks/comments/1qvie0r/installed_apps_enabled_smart_entryexit_with_key/) anuncia instalação em outro mercado, mas não publica um procedimento reproduzível ou inventário que demonstre equivalência com o H6 brasileiro. A classificação do sistema é discutida nos próprios comentários.

Essas pistas só passam a sustentar um porte quando houver identificação de modelo/mercado/build, diagnóstico do sistema e explicação verificável do acesso disponível.

## Resultados que não fornecem a base procurada

| Resultado | Motivo |
|---|---|
| [MyburghRoux/haval_h6_Infotainment](https://github.com/MyburghRoux/haval_h6_Infotainment) | Na revisão consultada, contém somente README com comandos genéricos e espaços para completar; não entrega o dump anunciado. O [relato associado](https://www.reddit.com/r/HavalTipsAndTricks/comments/1sjyepq/2021_haval_h6_4x4_super_lux_headunit_dump/) trata de H6 2021. |
| [luningning/haval-h6-infotainment-notes](https://github.com/luningning/haval-h6-infotainment-notes) | Notas, APKs e material identificado para H6 de segunda geração, incluindo script de maio de 2024. Sem identificação de Coffee OS 3 brasileiro nos materiais examinados. |
| [Garage Tool](https://garagetool.online/en/) | A própria página limita a evidência Haval ao F7/F7x, com método verificado no facelift de 2022 e F7 chinês de 2020. |
| [skwizzy222/haval-dargo-cluster](https://github.com/skwizzy222/haval-dargo-cluster/blob/main/package.json) | O manifesto descreve uma recriação visual do painel em uma página web. Não é firmware nem integração comprovada com a central. |
| [max-dtu/haval-docs](https://github.com/max-dtu/haval-docs) | Os documentos examinados são páginas de exemplo sem dados técnicos da central. |
| [gustavoclimaco/haval-port](https://github.com/gustavoclimaco/haval-port) | Projeto baseado em openpilot, voltado à assistência de direção; não fornece um porte da multimídia. |

## Próximo passo técnico

A prioridade é **identificar e obter uma coleta da unidade nova**. O procedimento inicial de consulta, condicionado a ADB já disponível, está no [documento de compatibilidade](COMPATIBILIDADE_H6_2026.md#coleta-inicial-se-já-houver-adb-autorizado).

| Evidência necessária | Decisão que ela permite tomar |
|---|---|
| Modelo/mercado, identificação da central e versões de software/MCU | Distinguir atualização da central antiga de nova plataforma e selecionar relatos realmente comparáveis. |
| Versão Android/API, plataforma e ABIs | Escolher requisitos do aplicativo e bibliotecas compatíveis. |
| Canal de diagnóstico disponível e permissões efetivas | Determinar se basta instalar um APK, se Shizuku é utilizável e quais recursos são acessíveis. Instalação não comprova acesso privilegiado. |
| Pacotes, serviços, descritores Binder/AIDL e metadados dos componentes pertinentes | Medir o reaproveitamento de rádio, launcher, clima e painel; definir a camada de compatibilidade. |
| Displays, resolução e densidade | Substituir associações fixas da geração anterior por descoberta de recursos e ajustar a interface. |

Sem essa coleta, alterar senhas, portas ou IDs no aplicativo seria especulação. Nenhum script dos projetos pesquisados foi executado, nenhum APK foi instalado e nenhum comando foi enviado ao veículo nesta pesquisa. Foram publicadas as conclusões e referências, sem novos dumps com identificadores pessoais.
