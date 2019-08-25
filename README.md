# Vagrant
Usando Vagrant para provisionar uma máquina virtual no virtualBox


Writing a Vagrantfile

Vagrantfiles são usados ​​para configurar máquinas virtuais vagantes. Este post vai um pouco além do básico de criar um Vagrantfile, e examina alguns dos tópicos mais avançados para escrever um arquivo vagrant para fazer uma máquina mais polida.

Eu postei anteriormente sobre como você pode provisionar sua máquina virtual através do arquivo vagrant - por exemplo, definir quais pacotes devem ser instalados quando a máquina é criada. Este post mostra como você também pode usar o arquivo vagrant para definir características da própria máquina, incluindo nomes personalizados de máquinas, memória e cpu, rede e pastas compartilhadas.

Nomeando a Máquina Vagrant
Quando você cria uma máquina virtual vagante, verá que o vagrant deu à máquina virtual um nome bastante genérico. Isso pode ser bom em alguns casos, mas você também pode querer ter mais controle sobre o nome da máquina virtual. Na verdade, existem algumas maneiras diferentes de interpretar o "nome da máquina":

vagrant tem seu próprio nome para a máquina virtual criada
o provedor (por exemplo, virtualbox) exibirá um nome para a máquina virtual
o "hostname" como aparece na linha de comando.
Nome próprio do Vagrant
Primeiramente, vamos dar uma olhada no nome vagrant usa para a máquina. O padrão é DIRECTORY_default_TIMESTAMP - geralmente isso é bom, mas você pode personalizá-lo. Você pode fazer isso com:

```ruby
 Vagrant.configure("2") do |config|
   config.vm.box = "hashicorp/precise64"
   config.vm.define "custom_vagrant_name"
end
```
Nome da GUI do provedor
O nome da máquina errante também pode aparecer no próprio provedor. Você também pode querer personalizar isso também. Isso será específico do provedor, portanto, você deve verificar os detalhes do seu provedor. Se você estiver usando caixa virtual, você pode usar:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
  config.vm.provider "virtualbox" do |v|
   v.gui = true
   v.name = "custom_vm_name"
  end
end
```
O nome do host
O hostname é o que sua máquina irá aparecer como em uma rede, e o que ele dirá na linha de comando quando você logar. O nome do host é limitado por regras ou convenções de SOEM, principalmente que podem ser letras separadas por hypen ou dot. Veja um exemplo:

```ruby
Vagrant.configure("2") do |config|
 config.vm.box = "hashicorp/precise64"
 config.vm.hostname = "my-vagrant-machine"
end
 ```

Configuração de memória e número de núcleos
Definir a configuração de hardware pode ser realmente útil, principalmente a memória e o número de núcleos / CPUs. Isso é outra coisa específica do provedor que você está usando.

Por exemplo, o provedor virtualbox pode ser configurado assim:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
 config.vm.provider "virtualbox" do |v|
   v.gui = true
   v.memory = 1024
   v.cpus = 2
  end
end
```
Abrindo / Encaminhando Portas
Você pode abrir portas / permitir o encaminhamento para permitir que você acesse uma porta em sua máquina host e tenha todos os dados encaminhados para uma porta na máquina convidada. Você configura o encaminhamento de porta com

```ruby
Vagrant.configure("2") do |config| 
 config.vm.box = "hashicorp/precise64"
 config.vm.network "forwarded_port", guest: 80, host: 8080
end
```
Compartilhando Pastas
A configuração de uma pasta compartilhada ou sincronizada facilita muito o trabalho com uma máquina virtual - você pode compartilhar arquivos entre o host e a máquina convidada. Você pode fazer isso no vagrant com a opção "synced_folder":

```ruby
Vagrant.configure("2") do |config|
 config.vm.box = "hashicorp/precise64"
 config.vm.synced_folder "local_folder", "/guest/folder"
end
```
Que sincronizará uma pasta "pasta_local" no mesmo diretório do arquivo "vagrantfile" com um arquivo em "/ home / vagrant / guest / folder". Você também pode explorar opções de sincronização de pastas mais avançadas, como compartilhamentos NFS, rsync e samba.

Juntando Tudo
Como um exemplo de alguns desses conceitos de arquivos vagrant mais avançados, aqui está um exemplo de construção oommf vagrant de referência.

```ruby
Vagrant.configure("2") do |config|
 config.vm.box = "hashicorp/precise64"
  
 config.vm.network "forwarded_port", guest: 80, host: 8080
 config.vm.hostname = "my-vagrant-machine"
 config.vm.define "custom_vagrant_name"
 config.vm.synced_folder "local_folder", "/guest/folder"
  
 config.vm.provider "virtualbox" do |v|
 v.gui = true
 v.name = "custom_vm_name"
 v.memory = 1024
 v.cpus = 2
 end
  
end
```


