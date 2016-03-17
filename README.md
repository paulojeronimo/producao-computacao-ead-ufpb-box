# Box vagrant para a produção de livros da UFPB

Este projeto pode ser utilizado na construção de [livros da UFPB](https://github.com/ufpb-computacao) que seguem o template para a construção de livros ([asciidoc-book-template-with-rake-and-github][livro-template]) na sua produção. Ele cria uma máquina virtual (tecnicamente, uma [box Vagrant](https://www.vagrantup.com/docs/boxes.html)) já contendo todas as ferramentas necessárias (instaladas e configuradas) para a produção de livros.

Atualmente, essa máquina virtual é executada pelo [VirtualBox][VirtualBox] e utiliza, como o sistema operacional, a distribuição Linux [Fedora 23][Fedora23]. Próximas versões deste projeto poderão possibilitar o uso de outros provedores de virtualização (como VMware, KVM, etc) executando, também, outras distribuições Linux (como Debian, Ubuntu, etc).

## Pré-requisitos

Antes de iniciar o uso deste projeto, você precisará instalar algumas ferramentas. Elas estão descritas nos subtópicos a seguir.

### Cygwin (se estiver utilizando o Windows)

Se você estiver utilizando o Windows, precisará de um interpretador Bash, e de um cliente SSH, para executar comandos apresentados neste documento. Uma forma de se instalar essas coisas é fazendo a instalação do Cygwin, conforme os [passos descritos pelo Paulo Jerônimo](seguinte://github.com/paulojeronimo/dicas-windows/blob/master/instalacao-cygwin.asciidoc).

### VirtualBox

Você precisará instalar o VirtualBox.

No Windows, uma das formas de instalá-lo é através do [Chocolatey](https://chocolatey.org/). Faça a sua instalação e, em seguida, execute o comando: 

    choco install virtualbox VirtualBox.ExtensionPack -y

No Fedora 23, a instalação do VirtualBox é detalhada [neste artigo do Paulo Jerônimo](https://github.com/paulojeronimo/dicas-linux/blob/master/instalacao-virtualbox.asciidoc).

### Vagrant

Neste projeto, o [Vagrant](https://vagrantup.com) será utilizado para provisionar (instalar/configurar as ferramentas necessárias) na máquina virtual que será executada pelo VirtualBox.

No Windows, de forma semelhante a instalação do VirtualBox, o Vagrant também pode ser instado através do Chocolatey. Utilize o seguinte comando:

    choco install vagrant -y

No Fedora 23, a instalação do Vagrant é detalhada [no site Fedora Developer](https://developer.fedoraproject.org/tools/vagrant/about.html).

#### vagrant-vbguest

O [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) é um plugin utilizado pelo [Vagrantfile](Vagrantfile) que, neste projeto, impede a atualização do [VirtualBox Guest Additions](https://www.virtualbox.org/manual/ch04.html) na box, caso o Vagrant detecte que ele está mais atual na versão do VirtualBox sendo utilizada. Impedir essa atualização evita problemas que poderiam ocorrer na montagem de diretórios compartilhados caso a box fosse atualizada.

Esse plugin pode ser instalado com o seguinte comando:

    vagrant plugin install vagrant-vbguest

## Como utilizar este projeto

Para construir a box de modo a utilizá-la na geração de seu livro, execute os seguintes passos:

    git clone https://github.com/paulojeronimo/producao-computacao-ead-ufpb-box
    cd producao-computacao-ead-ufpb-box
    vagrant up

Por _default_, este projeto já está configurado para provisionar uma máquina executando o Fedora 23. A box base utilizada é a [boxcutter/fedora23](https://atlas.hashicorp.com/boxcutter/boxes/fedora23) cujo projeto de construção está disponível no [GitHub](https://github.com/boxcutter/fedora). O provisionamento dessa base é realizado através de shell script, pelo script [provision/fedora23](provision/fedora23).

Para acessar a box e iniciar o processo de utilização do Rake para construir um livro, execute o seguinte comando:

    vagrant ssh

Sugerimos que quaisquer livros que forem ser construídos através deste projeto sejam baixados dentro do diretório `/vagrant/livros`. Esse diretório é compartilhado entre a tua máquina real e a box e, dessa forma, mesmo que a box seja destruída (com o comando `vagrant destroy`) você não perderá nenhum arquivo do repositório do teu livro.

Como exemplo, aqui estão os procedimentos para a construção do [livro template][livro-template] (utilizando o fork do Paulo Jerônimo):

    cd /vagrant/livros
    git clone https://github.com/paulojeronimo/asciidoc-book-template-with-rake-and-github
    cd asciidoc-book-template-with-rake-and-github
    rake

[livro-template]: https://github.com/ufpb-computacao/asciidoc-book-template-with-rake-and-github
[VirtualBox]: https://virtualbox.org
[Fedora23]: https://getfedora.org
