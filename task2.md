# Configurar e testar estratégias de envio


A relação entre [política de envio](https://help.vtex.com/pt/tutorial/politica-de-envio--tutorials_140), [doca](https://help.vtex.com/pt/tutorial/doca--5DY8xHEjOLYDVL41Urd5qj) e [estoque](https://help.vtex.com/pt/tutorial/estoque--6oIxvsVDTtGpO7y6zwhGpb) é o que define a estratégia de envio e a combinação de estrutura para a entrega dos pedidos da loja. 

Este artigo apresenta como você cria uma estratégia de envio pelo Admin VTEX e utiliza o [Simulador de envio](https://help.vtex.com/pt/tutorial/simulador-de-envio--tutorials_144?utm_term=&utm_campaign=BRA_pmax_2023&utm_source=adwords&utm_medium=ppc&hsa_acc=9663921675&hsa_cam=20809358286&hsa_grp=&hsa_ad=&hsa_src=x&hsa_tgt=&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad_source=1&gclid=Cj0KCQjwsJO4BhDoARIsADDv4vCbyHccKBf-W1vxG31B3hBXtNuyRC1WM80Op8k9fJ1zSAWcYaq520gaAvlTEALw_wcB) para testar o seu funcionamento. 

<p align="center">
  <img src="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExcnptbGczMXY1NjNiNTg1aDB1MWdiYWttYXZwZjdpdnc3OWYxYWE5eCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/LiO5VWDdLFa9l35Joy/giphy.gif" alt="Estratégia de envio" />
</p>

>⚠️ A quantidade de estratégias de envio que aparecem no checkout depende da política de envio, do [limite configurado pela loja](https://help.vtex.com/pt/tutorial/pontos-de-retirada--2fljn6wLjn8M4lJHA6HP3R#como-funcionam-pontos-de-retirada:~:text=Como%20funcionam%20pontos%20de%20retirada) e das configurações de [otimização das opções de entrega no checkout](https://help.vtex.com/pt/tutorial/otimizacao-das-opcoes-de-entrega-no-checkout--6DeGO9eBSFWe4XkoS0SxAB) A ordem de cadastro sugerida para uma estratégia de envio é:
> 1. [Política de envio](https://help.vtex.com/pt/tutorial/politica-de-envio--tutorials_140)
> 2. [Doca](https://help.vtex.com/pt/tutorial/doca--5DY8xHEjOLYDVL41Urd5qj)
> 3. [Estoque](https://help.vtex.com/pt/tutorial/estoque--6oIxvsVDTtGpO7y6zwhGpb)

Este artigo contém as informações necessárias para:
* Cadastrar uma política comercial
* Cadastrar doca
* Cadastrar estoque
* Testar estratégia de envio

## Cadastrar uma política de envio

Esta etapa define o conjunto de regras e configurações que estabelece quais opções e condições de envio serão exibidas aos clientes no momento do checkout.

Os campos obrigatórios para a configuração de uma política de envio funcional são: `nome`, `método de envio`, `upload da tabela de frete` e `horário de funcionamento`. Mais detalhes sobre as demais opções de configuração, consulte [Criar uma política de envio](https://help.vtex.com/pt/tutorial/criar-uma-politica-de-envio--66rJO4LKBdyMJOH6Z3dsaT).

Para criar uma nova política de envio, siga os passos abaixo:
1. No Admin VTEX, acesse **Envio > Estratégia de Envio > Políticas de Envio** ou digite *Estratégia de Envio* na barra de busca no topo da página.
2. Clique no botão `Criar política de envio`.
3. Preencha os campos da tela descritos abaixo e, em seguida, clique em `Salvar alterações`.
* **Nome:** nome interno da política de envio.
* **Método de Envio:** nome do tipo de entrega. Serve para separar os diferentes serviços de entrega. É o nome que será exibido na finalização de compra para o cliente. Ex. expresso vs. normal.
* **Upload da tabela de frete:** é a tabela com os valores e prazos de frete assumidos pela política de envio. Para mais detalhes, consulte [Planilha de frete](https://help.vtex.com/pt/tutorial/planilha-de-frete--tutorials_127).
* **Horário de funcionamento:** os horários em que a transportadora faz coletas e as janelas de tempo em que ela envia os itens para os clientes. Estas configurações influenciam o cálculo do tempo de entrega.

## Cadastrar doca

A doca é uma etapa intermediária da logística que conecta o estoque à transportadora, facilitando o fluxo de produtos até o destinatário final. Na VTEX, ela integra armazenamento, [políticas comerciais](https://help.vtex.com/pt/tutorial/como-funciona-uma-politica-comercial--6Xef8PZiFm40kg2STrMkMV) e de envio, atuando como ponto essencial para a eficiência na entrega dos pedidos.

Para o funcionamento da doca é obrigatória a configuração dos campos: `nome` e `políticas comerciais e de envio`. 

Para cadastrar uma doca, siga os passos abaixo: 
1. No Admin VTEX, acesse **Envio > Estratégia de Envio** ou digite *Estratégia de envio* na barra de busca no topo da página.
2. Vá na aba **Docas** e clique no botão `Criar Doca`.
3. Preencha os campos abaixo e clique em `Salvar`.
* **Nome:** o nome da doca.
* [Políticas de envio](https://help.vtex.com/pt/tutorial/politica-de-envio--tutorials_140) e [Política comercial](https://help.vtex.com/pt/tutorial/como-funciona-uma-politica-comercial--6Xef8PZiFm40kg2STrMkMV): selecione a política de envio e política comercial que atendem a doca. É importante ressaltar que ambas políticas devem ter sido cadastradas anteriormente.

Para mais detalhes sobre os campos de cadastro, consulte [Gerenciar Doca](https://help.vtex.com/pt/tutorial/gerenciar-doca--7K3FultD8I2cuuA6iyGEiW).

## Cadastrar estoque

O estoque é o espaço físico de armazenamento no e-commerce, essencial para controlar a movimentação e a disponibilidade dos produtos. Na VTEX, você pode adicionar unidades ao inventário dos estoques cadastrados e gerenciar seu envio por meio das docas e políticas de envio. 

Para cadastrar um novo estoque, siga os passos abaixo:
1. No Admin VTEX, acesse **Envio > Estratégia de Envio** ou digite *Estratégia de envio* na barra de busca no topo da página.
2. Clique na aba `Estoques`.
3. Clique no botão `Criar estoque`.
4. Preencha o campo **Nome** com o nome do estoque.
5. Clique em `Adicionar doca`, selecione uma das docas previamente cadastradas e clique em `Salvar Alterações`.

>⚠️ Um tempo de processamento pode ser cadastrado em cada uma das etapas da estratégia de envio. Para mais detalhes consulte [Como é calculado o prazo de entrega do pedido](https://help.vtex.com/pt/tutorial/como-e-calculado-o-prazo-de-entrega-do-pedido--1TOuKCIjGQmqOqQkEqCg82).

## Testar estratégia de envio

O **Simulador de Envio** disponível no Admin VTEX permite ao lojista simular e analisar opções de entrega, verificando disponibilidade de itens, formas de entrega, custos e prazos. Ele ajuda a validar configurações de estratégia de envio antes de habilitar o processo logístico, considerando a disponibilidade de estoque e transportadoras para a região.

Para realizar uma simulação, siga os passos abaixo preenchendo os campos obrigatórios: 
1. Selecione o `País` e `Política comercial` desejados.
2. Preencha o campo ` Selecione um produto` com o nome ou ID de SKU.
3. Preencha o campo `Quantidade` do SKU.
4. Digite o **CEP** que deseja realizar a simulação.
5. Clique no botão `Simular Frete`

> ℹ️ O campo `Preço` é opcional e o simulador não leva em consideração o preço cadastrado no módulo Catálogo.

>⚠️ Para entender mais detalhes sobre a simulação e os motivos mais comuns que fazem com que uma transportadora não esteja disponível para simulação, consulte [Simulador de Envio](https://help.vtex.com/pt/tutorial/simulador-de-envio--tutorials_144?utm_term=&utm_campaign=BRA_pmax_2023&utm_source=adwords&utm_medium=ppc&hsa_acc=9663921675&hsa_cam=20809358286&hsa_grp=&hsa_ad=&hsa_src=x&hsa_tgt=&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad_source=1&gclid=Cj0KCQjwsJO4BhDoARIsADDv4vCbyHccKBf-W1vxG31B3hBXtNuyRC1WM80Op8k9fJ1zSAWcYaq520gaAvlTEALw_wcB).
