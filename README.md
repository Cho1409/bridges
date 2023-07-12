Documentação: Utilizando o ARGO CD em AWS EKS
Este guia irá ajudá-lo a utilizar o ARGO CD (Continuous Deployment) em um cluster EKS (Elastic Kubernetes Service) da AWS (Amazon Web Services). O ARGO CD é uma ferramenta de CI/CD (Continuous Integration/Continuous Deployment) projetada para facilitar o processo de implantação contínua de aplicações em clusters Kubernetes.

Pré-requisitos
Antes de começar, você precisa ter os seguintes pré-requisitos em vigor:

Uma conta AWS com acesso ao serviço EKS.
Um cluster EKS configurado e em execução.
O cliente de linha de comando kubectl instalado e configurado para se conectar ao cluster EKS.
Passo 1: Instalando o ARGO CD
Abra o terminal e certifique-se de que o kubectl está configurado corretamente para se conectar ao cluster EKS.

Execute o seguinte comando para instalar o ARGO CD:

bash
Copy code
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Isso criará um novo namespace chamado argocd e instalará todos os componentes do ARGO CD no cluster.

Verifique se os pods do ARGO CD estão em execução executando o seguinte comando:

bash
Copy code
kubectl get pods -n argocd
Certifique-se de que todos os pods estejam em estado "Running" antes de prosseguir.

Passo 2: Acessando a interface do ARGO CD
Execute o seguinte comando para expor o serviço web do ARGO CD como um serviço externo:

bash
Copy code
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
Espere alguns minutos até que o serviço seja provisionado e um endereço IP externo seja atribuído a ele.

Para obter o endereço IP externo do ARGO CD, execute o seguinte comando:

bash
Copy code
kubectl get svc argocd-server -n argocd -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
Anote o endereço IP retornado.

Abra um navegador da web e acesse o endereço IP externo do ARGO CD. Você será redirecionado para a interface de login.

Use as seguintes credenciais para fazer login:

Nome de usuário: admin

Senha: execute o seguinte comando para obter a senha padrão:

bash
Copy code
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
Copie a senha gerada e cole-a na interface de login.

Passo 3: Configurando a integração com o cluster EKS
Na interface do ARGO CD, clique em "Settings" (Configurações) no canto superior direito.

Na seção "Repositories" (Repositórios), clique em "New Repository" (Novo Repositório).

Preencha os detalhes do repositório Git que contém seus manifestos Kubernetes e clique em "Save" (Salvar). Certifique-se de fornecer as credenciais corretas, se necessário.

Volte para a página inicial do ARGO CD e você verá seu repositório listado na seção "Applications" (Aplicações).

Clique em "New Application" (Nova Aplicação) para criar uma nova aplicação.

Preencha os detalhes da aplicação, como nome, projeto e origem do repositório. Certifique-se de fornecer o caminho para seus manifestos Kubernetes corretamente.

Na seção "Sync Policy" (Política de Sincronização), você pode definir quando e como a sincronização da aplicação ocorrerá. A configuração padrão é a mais comum, mas você pode personalizá-la de acordo com suas necessidades.

Clique em "Create" (Criar) para criar a aplicação.

Passo 4: Implantação contínua com o ARGO CD
Agora que você configurou o ARGO CD e criou uma aplicação, ele iniciará automaticamente o processo de implantação contínua com base nos manifestos Kubernetes fornecidos.

Na interface do ARGO CD, vá para a página inicial e você verá a lista de suas aplicações.

Clique na aplicação que você criou e você será redirecionado para a página de detalhes da aplicação.

Nesta página, você pode ver o status da sincronização, histórico de sincronizações anteriores, recursos implantados e muito mais.

Se você fizer alterações nos manifestos Kubernetes no repositório Git, o ARGO CD detectará automaticamente as alterações e iniciará uma nova sincronização para implantar as atualizações.

Parabéns! Agora você está pronto para usar o ARGO CD em seu cluster EKS da AWS para realizar implantações contínuas de suas aplicações Kubernetes de forma fácil e automatizada.

Esta documentação forneceu uma visão geral básica de como utilizar o ARGO CD com o EKS. Para uma compreensão mais aprofundada e personalização, consulte a documentação oficial do ARGO CD e da AWS EKS.
