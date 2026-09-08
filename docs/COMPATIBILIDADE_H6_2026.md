# Compatibilidade com a nova central do Haval H6

Análise em 08/09/2026. Alvo provisório: H6 brasileiro com central de 14,6 polegadas e Coffee OS 3. A versão do veículo e o firmware da unidade a testar ainda precisam ser informados.

## Resultado

Este repositório é uma referência de diagnóstico da central antiga. **Não contém um aplicativo, projeto compilável ou firmware que possa ser atualizado para funcionar no H6 novo.** Para torná-lo útil à nova geração, é necessário acrescentar uma coleta identificada do Coffee OS 3 e documentar as diferenças. Implementar personalizações exige outro projeto de aplicativo, desenvolvido ou adaptado com base nessa coleta.

A adaptação do aplicativo relacionado `bobaoapae/haval-app-tool-multimidia` (Impulse/HavalShisuku) envolve pelo menos acesso ao sistema, interfaces dos serviços e descoberta dos displays. O código consultado depende explicitamente da plataforma antiga. Não foi encontrado suporte confirmado ao Coffee OS 3 nas fontes examinadas; isso não demonstra impossibilidade técnica.

## Evidências verificadas

Base deste fork: commit [`e246a3de5c7253c0942db3a9a9bed6554b47e75b`](https://github.com/ipsBruno/gwm-haval-multimidia-native/tree/e246a3de5c7253c0942db3a9a9bed6554b47e75b), de 23/08/2024. São cinco arquivos versionados: `README.md`, `getprops.txt`, `packages.txt`, `dumpsys.txt` e `apks/readme.md`. A pasta `apks` contém somente esse README de dois bytes; não há APKs nessa revisão.

| Item | Coleta antiga | Central nova / consequência |
|---|---|---|
| Sistema Android | Android 9, API 28; build de 29/12/2022; patch informado de 05/02/2020 | Versão Android, API e patch do Coffee OS 3 não foram verificados. Não deduzir esses valores pelo nome comercial. |
| Plataforma | `ro.board.platform=msmnile`, hardware `qcom`, ABIs ARM64 e ARM32 | SoC, fabricante e ABIs precisam ser obtidos na unidade nova. |
| Serviços | Componentes Bosch, Beantechs e Autolink; `car_service` presente | Nomes, contratos e permissões podem ter mudado; conferir antes de reutilizar chamadas. |
| Pacotes | Launcher/HVAC Beantechs, cluster Autolink, Android Auto/CarPlay `com.ts.*` | Inventariar os pacotes novos e suas versões. |
| Acesso | README antigo registra `adbd` parado e `busybox-telnetd` ativo; há listener BusyBox em 9090 | É um registro histórico, não uma instrução nem evidência de acesso na central nova. |
| Interface | A GWM informa que a tela anterior era de 12,3 polegadas | O novo H6 usa tela de 14,6 polegadas Full HD, Coffee OS 3 e barra personalizável persistente. Densidade, área útil e displays Android ainda precisam ser medidos. |

Fontes locais: [propriedades](../getprops.txt), [pacotes](../packages.txt), [diagnóstico](../dumpsys.txt) e [README original](../README.md). A mudança de tela e plataforma é confirmada no [comunicado da GWM sobre o novo H6](https://www.gwmmotors.com.br/pt/media-center/news/2025/gwm-apresenta-o-novo-haval-h6-adaptado-ao-gosto-dos-motoristas-brasileiros).

Na [issue 46 do Impulse](https://github.com/bobaoapae/haval-app-tool-multimidia/issues/46), os comentários de [19/03/2026](https://github.com/bobaoapae/haval-app-tool-multimidia/issues/46#issuecomment-4092901846) e [24/04/2026](https://github.com/bobaoapae/haval-app-tool-multimidia/issues/46#issuecomment-4312335382) relatam ausência de suporte ao Coffee OS 3. São relatos de participantes, não uma certificação da GWM ou um teste feito nesta análise. Os sete comentários disponíveis foram consultados pela API; a renderização resumida da página não os mostrava. A issue continuava aberta.

## Dependências concretas do aplicativo relacionado

Código consultado na revisão [`3c856462791fe9c6248358ab83901e618de4c149`](https://github.com/bobaoapae/haval-app-tool-multimidia/tree/3c856462791fe9c6248358ab83901e618de4c149), de 25/08/2026. Estes arquivos pertencem ao Impulse, não ao presente fork.

| Parte | Dependência observada | Trabalho necessário para um porte |
|---|---|---|
| [ForegroundService.java](https://github.com/bobaoapae/haval-app-tool-multimidia/blob/3c856462791fe9c6248358ab83901e618de4c149/app/src/main/java/br/com/redesurftank/havalshisuku/services/ForegroundService.java#L80) | Verificação de UID, Telnet local na porta 23 e procura de `libshizuku.so` | Separar a inicialização da estratégia antiga de acesso; verificar que permissões a nova unidade efetivamente oferece. Acesso ADB e acesso aos serviços privilegiados são verificações distintas. |
| [ServiceManager.java](https://github.com/bobaoapae/haval-app-tool-multimidia/blob/3c856462791fe9c6248358ab83901e618de4c149/app/src/main/java/br/com/redesurftank/havalshisuku/managers/ServiceManager.java#L285) | Serviços Beantechs, pool de binders com IDs 6/8/13, cluster Autolink e serviço de entrada | Comparar descritores AIDL/Binder, métodos, permissões e significado das propriedades. Um nome igual não garante contrato igual. |
| [ProjectorManager.java](https://github.com/bobaoapae/haval-app-tool-multimidia/blob/3c856462791fe9c6248358ab83901e618de4c149/app/src/main/java/br/com/redesurftank/havalshisuku/managers/ProjectorManager.java) | Projetores associados a displays específicos da central antiga | Descobrir telas por suas características e permissões; ajustar layout, densidade, insets e prioridade da interface nativa. |

Mudar apenas resolução, senha, porta ou versão de SDK não estabelece compatibilidade. A porta 9090 no snapshot e a porta 23 no Impulse são observações de contextos diferentes e não comprovam qual acesso existe no Coffee OS 3.

## Sequência de trabalho

1. **Identificar a unidade.** Registrar versão HEV/PHEV/GT, ano-modelo, mercado, modelo da central e versões de software/MCU. O ano do carro sozinho não identifica o firmware.
2. **Obter diagnóstico por uma interface disponível e autorizada.** Confirmar se há ADB ou outro canal de diagnóstico. Sem isso, o levantamento fica restrito às informações da tela e aos materiais do fabricante. Este fork não fornece um desbloqueio para Coffee OS 3.
3. **Produzir o inventário novo.** Registrar propriedades selecionadas, pacotes, serviços e displays; guardar a coleta separada da de 2024. Se houver acesso aos arquivos do sistema, extrair metadados das interfaces pertinentes para comparação local.
4. **Comparar os contratos.** Identificar o que existe na nova plataforma e quais operações podem ser usadas com as permissões disponíveis. Separar atalhos visuais de integração com o painel e comandos do veículo.
5. **Implementar em um aplicativo.** Criar uma camada por versão da central, com detecção de recursos e desativação das funções não suportadas. O presente repositório pode documentar a comparação, mas não contém a implementação a portar.
6. **Validar na unidade real.** Começar com um app de diagnóstico que apenas consulta informações. Para a interface, conferir inicialização, suspensão/retomada, áudio, Android Auto/CarPlay, câmera, climatização e preservação dos avisos nativos. Nenhum desses testes foi realizado aqui.

## Coleta inicial, se já houver ADB autorizado

Comandos de consulta para execução futura no computador, com o carro estacionado. Substituir `SERIAL_DA_CENTRAL` pelo identificador da unidade autorizada. Estes comandos não foram executados nesta análise e não habilitam ADB, root ou permissões.

```sh
mkdir -p captures/h6-coffeeos3-local
for prop in ro.build.version.release ro.build.version.sdk \
  ro.build.version.security_patch ro.build.display.id \
  ro.product.manufacturer ro.product.model ro.board.platform \
  ro.product.cpu.abilist ro.debuggable ro.secure; do
  printf '%s=' "$prop"
  adb -s SERIAL_DA_CENTRAL shell getprop "$prop"
done > captures/h6-coffeeos3-local/properties-selected.txt
adb -s SERIAL_DA_CENTRAL shell pm list packages > captures/h6-coffeeos3-local/packages.txt
adb -s SERIAL_DA_CENTRAL shell service list > captures/h6-coffeeos3-local/services.txt
adb -s SERIAL_DA_CENTRAL shell dumpsys display > captures/h6-coffeeos3-local/display.txt
adb -s SERIAL_DA_CENTRAL shell wm size > captures/h6-coffeeos3-local/screen-size.txt
adb -s SERIAL_DA_CENTRAL shell wm density > captures/h6-coffeeos3-local/screen-density.txt
```

Registrar comandos recusados como limitação da coleta; arquivo vazio não significa ausência do recurso. A pasta `/captures/` está ignorada pelo Git. Os dumps antigos contêm identificadores do veículo e de dispositivos; novas coletas devem permanecer locais até remoção desses dados. Publicar apenas a comparação necessária.

## Estado da entrega

Fork e clone preparados, inventário antigo examinado, dependências do aplicativo relacionado identificadas e plano de avaliação documentado. Nenhum firmware, APK ou configuração de veículo foi alterado. A compatibilidade prática e as mudanças exatas dependem do inventário da nova unidade.
