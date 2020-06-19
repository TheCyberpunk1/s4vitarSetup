# s4vitarSetup
Instalando el entorno de @S4vitar paso a paso.

# s4vitar configuración
No busco guiar a nadie ni hacer una explicación, es solo por si tengo que aplicar la misma configuración en otra maquina.
 Pero si a alguien le sirve lo dejo publico.
 Me ha funcionado en **Parrot 4.9** y **Kali Linux 2020.2**.

Si buscas instalar automáticamente mira este script de @kyb3r-bat [https://github.com/kyb3r-bat/Kyb3rvotarOS](https://github.com/kyb3r-bat/Kyb3rvotarOS)

Si buscas seguir el vídeo de @s4vitar [https://www.youtube.com/watch?v=MF4qRSedmEs](https://www.youtube.com/watch?v=MF4qRSedmEs)
 

**Actualizar Parrot .**

    apt update

**Ejecutar los siguientes comandos en usuario root**
 
**Instalar Bpswm.**

`apt-get install bspwm -y` 
 Instalar dependencias que encuentras aquí.  [https://github.com/baskerville/bspwm/wiki](https://github.com/baskerville/bspwm/wiki "https://github.com/baskerville/bspwm/wiki")

    sudo apt-get install libxcb-xinerama0-dev libxcb-icccm4-dev libxcb-randr0-dev libxcb-util0-dev libxcb-ewmh-dev libxcb-keysyms1-dev libxcb-shape0-dev

**Ahora como usuario normal.**  

    git clone https://github.com/baskerville/bspwm.git  
    git clone https://github.com/baskerville/sxhkd.git 
    cd bspwm && make && sudo make install  
    cd ../sxhkd && make && sudo make install  
    mkdir -p ~/.config/{bspwm,sxhkd}  
    cp /usr/local/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/  
    cp /usr/local/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/ 
    chmod u+x ~/.config/bspwm/bspwmrc

**Editamos el xinitrc para que arranque el escritorio de bspmw.**

`nano ~/.xinitrc`

 **Pegamos el siguiente código** 
 
 

    sxhkd & 
    exec bspwm
      

guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Instalamos compton (transparencia) , feh (fondo de pantalla) y rofi (lanzador de aplicaciones)** 

    sudo apt-get install compton -y
    sudo apt-get install feh -y
    sudo apt-get install rofi -y


Los archivos de configuración que se usaran en esta guía los puedes encontrar en  [https://s4vitar.github.io/bspwm-configuration-files/#](https://s4vitar.github.io/bspwm-configuration-files/#).


**Nota.**
Modificas los nombres de usuario también creas una carpeta en el escritorio o le das la ruta a la **linea feh** para especificar donde se encuentra tu fondo de pantalla o si quieres solo agregas las lineas. 
También tener en cuenta que en la linea **pointer_modifier mod1** puede variar por mod2 mod3 mod4 depende la configuración de tu escritorio.

    cd ~/.config/bspwm
    nano bspwmrc

**Borras el código y pegar este en el archivo bspwmrc**

    #! /bin/sh  
          
    sxhkd &
    compton --config /home/usuario/.config/compton/compton.conf &   
    feh --bg-fill /home/usuario/Desktop/Images/Helado.jpg &
    
    wmname LG3D &
    
    ~/.config/polybar/launch.sh &   
    
    bspc config pointer_modifier mod1   
    
    bspc monitor -d I II III IV V VI VII VIII IX X   
          
    bspc config border_width 2
    bspc config window_gap 12
    bspc config split_ratio 0.52
    bspc config borderless_monocle true
    bspc config gapless_monocle true
   
    bspc rule -a Caja desktop='^8'  state=floating follow=on
    bspc rule -a Gimp desktop='^8' state=floating follow=on
    bspc rule -a Chromium desktop='^2'
    bspc rule -a mplayer2 state=floating
    bspc rule -a Kupfer.py focus=on
    bspc rule -a Screenkey manage=off
    
    bspc rule -a burp-StartBurp: state=floating

guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Copiar el archivo de configuración sxhkdrc de s4vitar**

    cd ~/.config/sxhkd
    nano sxhkdrc
Borrar el contenido y pegar este.


    #
    # wm independent hotkeys
    #
    
    # terminal emulator
    super + Return
    	gnome-terminal
    
    # program launcher
    super + d
    	rofi -show run
    
    # make sxhkd reload its configuration files:
    super + Escape
    	pkill -USR1 -x sxhkd
    
    #
    # bspwm hotkeys
    #
    
    # quit/restart bspwm
    super + alt + {q,r}
    	bspc {quit,wm -r}
    
    # close and kill
    super + {_,shift + }w
    	bspc node -{c,k}
    
    # alternate between the tiled and monocle layout
    super + m
    	bspc desktop -l next
    
    # send the newest marked node to the newest preselected node
    super + y
    	bspc node newest.marked.local -n newest.!automatic.local
    
    # swap the current node and the biggest node
    super + g
    	bspc node -s biggest
    
    #
    # state/flags
    #
    
    # set the window state
    super + {t,shift + t,s,f}
    	bspc node -t {tiled,pseudo_tiled,floating,fullscreen}
    
    # set the node flags
    super + ctrl + {m,x,y,z}
    	bspc node -g {marked,locked,sticky,private}
    
    #
    # focus/swap
    #
    
    # focus the node in the given direction
    #super + {_,shift + }{h,j,k,l}
    #	bspc node -{f,s} {west,south,north,east}
    
    super + {_,shift + }{Left,Down,Up,Right}
           bspc node -{f,s} {west,south,north,east}
    
    
    # focus the node for the given path jump
    super + {p,b,comma,period}
    	bspc node -f @{parent,brother,first,second}
    
    # focus the next/previous node in the current desktop
    super + {_,shift + }c
    	bspc node -f {next,prev}.local
    
    # focus the next/previous desktop in the current monitor
    super + bracket{left,right}
    	bspc desktop -f {prev,next}.local
    
    # focus the last node/desktop
    super + {grave,Tab}
    	bspc {node,desktop} -f last
    
    # focus the older or newer node in the focus history
    super + {o,i}
    	bspc wm -h off; \
    	bspc node {older,newer} -f; \
    	bspc wm -h on
    
    # focus or send to the given desktop
    super + {_,shift + }{1-9,0}
    	bspc {desktop -f,node -d} '^{1-9,10}'
    
    #
    # preselect
    #
    
    # preselect the direction
    super + ctrl + alt + {Left,Down,Up,Right}
    	bspc node -p {west,south,north,east}
    
    
    # preselect the ratio
    super + ctrl + {1-9}
    	bspc node -o 0.{1-9}
    
    # cancel the preselection for the focused node
    super + ctrl + space
    	bspc node -p cancel
    
    # cancel the preselection for the focused desktop
    super + ctrl + alt + space
    	bspc query -N -d | xargs -I id -n 1 bspc node id -p cancel
    
    #
    # move/resize
    #
    
    # expand a window by moving one of its side outward
    #super + alt + {h,j,k,l}
    #	bspc node -z {left -20 0,bottom 0 20,top 0 -20,right 20 0}
    
    # contract a window by moving one of its side inward
    #super + alt + shift + {h,j,k,l}
    #	bspc node -z {right -20 0,top 0 20,bottom 0 -20,left 20 0}
    
    # move a floating window
    super + ctrl + {Left,Down,Up,Right}
    	bspc node -v {-20 0,0 20,0 -20,20 0}
    
    # Custom move/resize
    alt + super + {Left,Down,Up,Right}
    	/home/nombre de usuario/.config/bspwm/scripts/bspwm_resize {west,south,north,east}
    
    
    # ---------------------------------------------
    # CUSTOM
    # ---------------------------------------------
    
    # Google-Chrome
    
    super + shift + g
    	google-chrome
    
    # Open Burpsuite Professional
    
    super + ctrl + b
    	gksudo burp
guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Crear carpeta scripts en bspwm**

    cd ~/.config/bspwm
    mkdir scripts
    cd scripts
    touch bspwm_resize
    chmod +x bspwm_resize

**pegamos la configuración de s4vitar en el archivo bspwm_resize**

    nano bspwm_resize
**pegar**

    #!/usr/bin/env dash
    
    if bspc query -N -n focused.floating > /dev/null; then
    	step=20
    else
    	step=100
    fi
    
    case "$1" in
    	west) dir=right; falldir=left; x="-$step"; y=0;;
    	east) dir=right; falldir=left; x="$step"; y=0;;
    	north) dir=top; falldir=bottom; x=0; y="-$step";;
    	south) dir=top; falldir=bottom; x=0; y="$step";;
    esac
    
    bspc node -z "$dir" "$x" "$y" || bspc node -z "$falldir" "$x" "$y"

guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Reiniciar sesión**

    kill -9 -1
   
  iniciamos sesión con **bspwm**, lo elegimos en la parte superior del login.

 
Una vez se inicie sesión, no se vera nada, para ella damos **windows + enter** de esta manera se abrirá una terminal.


**Crear el archivo de configuración para transparencia con compton**

    cd /home/usuario/.config
    mkdir compton
    cd !$ >> si no funciona >> cd compton.
    touch compton.conf
Modificar el archivo.

    nano ~/.config/compton/compton.conf
pegamos.

    active-opacity = 0.95;
    inactive-opacity = 0.80;
    frame-opacity = 0.80;
    
    backend = "glx";
    
    opacity-rule = [
    	"50:class_g = 'Bspwm' && class_i = 'presel_feedback'",
    	"80:class_g = 'Rofi'",
    	"80:class_g = 'Caja'",
    	"99:class_g = 'Google-chrome'",
    "99:class_g = 'Firefox'",
    	
    	"99:class_g = 'burp-StartBurp'"
    ];
    
    blur-background = true;

guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Configurar java.**

    sudo su
    update-alternatives --config java
    >>opcion 2
**Instalar Polybar**
 [https://github.com/polybar/polybar](https://github.com/polybar/polybar) [https://github.com/polybar/polybar/wiki/Compiling](https://github.com/polybar/polybar/wiki/Compiling)

    apt install build-essential git cmake cmake-data pkg-config python3-sphinx libcairo2-dev libxcb1-dev libxcb-util0-dev libxcb-randr0-dev libxcb-composite0-dev python3-xcbgen xcb-proto libxcb-image0-dev libxcb-ewmh-dev libxcb-icccm4-dev

Si te falla, hacer `apt-get update`

Descargar en releases la version 3.4.0, ya que da errores.

[https://github.com/polybar/polybar/releases](https://github.com/polybar/polybar/releases)

**Instalar dependencias**

    apt install libxcb-xkb-dev libxcb-xrm-dev libxcb-cursor-dev libasound2-dev libpulse-dev i3-wm libjsoncpp-dev libmpdclient-dev libcurl4-openssl-dev libnl-genl-3-dev
    
**Configuramos.**

    cd /opt
    git clone https://github.com/polybar/polybar/

**Ejecutar como usuario root.**

    cd /opt/
    mv /home/usuario/Downloads/polybar-3.4.0.tar .
    tar -xf polybar-3.4.0.tar
    rm polybar-3.4.0.tar
    cd polybar
    mkdir build
    cd build
    cmake ..
    make -j$(nproc)
    make install

**Descargar la tipografia.**
Descargar Hack nerd fonts. [nerdfonts.com](nerdfonts.com)
https://github.com/ryanoasis/nerd-fonts/releases/download/v2.1.0/Hack.zip

    sudo su
    cd /usr/local/share/fonts
    mv /home/usuario/Downloads/Hack.zip
    unzip Hack.zip
    rm Hack.zip
    
**Configurar polybar como usuario normal no root**

    cd ~/.config/
    mkdir polybar
    cd polybar
**Creamos un script**

    touch launch.sh
    chmod +x launch.sh
    nano launch.sh
**Pegamos el siguiente código que se encuentra en la pagina de s4vitar**

    #!/bin/bash
    
    # Terminate already running bar instances
    killall -q polybar
    
    # Wait until the processes have been shut down
    while pgrep -u $UID -x polybar >/dev/null; do sleep 1; done
    
    # Launch bar1 and bar2
    polybar example &
    
    echo "Polybar launched..."


Guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Hacemos un updatedb como root**

    updatedb
hacemos una consulta.

    locate config | grep polybar
    cp /opt/polybar/config
    chown usuario:usuario -R *

**Windows alt + r** reiniciar el escritorio o **windows alt + q**.

**Ahora para modificar el estilo de como se ve la polybar pueden ir al vídeo de s4vitar en el minuto 1:08:25**
[https://www.youtube.com/watch?v=MF4qRSedmEs](https://www.youtube.com/watch?v=MF4qRSedmEs)

**O seguir estas lineas**

    cd ~/.config/polybar
    nano config

Modificas lo siguiente.

Los iconos lo consigues en [https://www.nerdfonts.com/cheat-sheet](https://www.nerdfonts.com/cheat-sheet) 

**escribes circle.**

label-focused = %index% lo cambias por label-focused = aqui el icono

label-occupied = %index% lo cambias por label-occupied = aqui el icono

label-urgent = %index$! Lo cambias por label-urgent = aqui el icono

label-empty = %index! Lo cambias por label-empty = aqui el icono

La linea label focused underline la puedes comentar para quitar el efecto amarillo de la barra

También puedes quitar las underline de los módulos comentándolo con un #
  

**Para que acepte los iconos debes agregar la fuente**

Buscas dentro del mismos archivo donde dice font 1 font 2 y agregas esta justo debajo

font-3 = Hack Nerd Font Mono
  

**Para configurar la barra superior**

En la linea modules center puedes cambiarla

modules-center = xwindow

modules-right = quitar los que no consideres necesarios.

**Configurar nuestros propios módulos, en este caso siguiendo el ejemplo de @s4vitar**
Como usuario normal no root.

    cd ~/.config
    mkdir bin
    cd bin
    touch ethernet_status.sh
    chmod +x ethernet_status.sh
    nano ethernet_status.sh

**En esta parte del código seguir en el video 1 h: 17min**
**Pegar el siguiente código que s4vitar publico**
  

    # Configuración ethernet para el Polybar
    #!/bin/sh
    echo "%{F#2495e7} %{F#e2ee6a}$(/usr/sbin/ifconfig eth0 | grep "inet " | awk '{print $2}')%{u-}"

Guardamos **ctrl + o** [enter] y luego **ctrl + x**.

Para ejecutar editar el archivo de configuración de polybar

    nano ~/.config/polybar/config
**Ejemplo:** Agregar un modulo nuevo al final - pegar lo siguiente = 

    [module/ethernet]    
    Type = custom/script
    Exec = ~/.config/bin/ethernet_status.sh
 
en la parte de la barra donde muestras los módulos agregar el nombre

**Ejemplo:** `modules-right = agregar ethernet` junto a los que tengas

Para agregar el icono de vpn hacer el mismo procedimiento dentro de **config/bin**

    cd ~/.config/bin
    touch vpnstatus.sh
    chmod +x vpnstatus.sh
    nano vpnstatus.sh
Pegar.

    IFACE=$(/usr/sbin/ifconfig | grep tun0 | awk '{print $1}' | tr -d ':')
    if [ "$IFACE" = "tun0" ]; then
    	echo "%{F#1bbf3e}icono %{F#e2ee6a}$(/usr/sbin/ifconfig tun0 | grep "inet " | awk '{print $2}' )%{u-}"
    else
    	echo "%{F#1bbf3e}icono%{u-}%{F-}"
    Fi


Guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Guardar y para ejecutar editar el archivo de configuración de polybar**.

    nano ~/.config/polybar/config

**Ejemplo:** Agregar un modulo nuevo al final.

    [module/vpn]
    type = custom/script
    exec = ~/.config/bin/ethernet_status.sh
 

En la parte de la barra donde muestras los módulos agregar el nombre

**Ejemplo:** `modules-right = agregar vpn` junto a los que tengas.

  
Puedes probarlo corriendo la vpn de hackthebox.
 

**Para volver transparente la polybar modificar al principio del archivo.
Como usuario normal no root.**

    nano ~/.config/polybar/config

  
En el archivo cambiar el color del background por `#aa2F343F`.

**Instalar crackmapexec** [https://github.com/byt3bl33d3r/CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)

    cd /opt/
    sudo git clonet https://github.com/byt3bl33d3r/CrackMapExec


**Instalar zsh**

    sudo su
    apt-get install zsh -y

**instalar power level 10** [https://github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k)

**Como usuario normal no root**
 

    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k

    echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc


**Ejecutar zsh**

Configurar a tu gusto o ver el vídeo de s4vitar si lo quieres exactamente igual hora **1:30:58**.

Si quieres configurar la zsh el archivo, a tu-gusto.

    nano ~/.p10k.zsh


**aplicar zsh a usuario root**

    sudo su
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
    echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>! ~/.zshrc

  
Configurar igual a tu gusto y cambiar el mensaje en el contexto 
por un icono.

    nano ~/.p10k.zsh

  
**Cambiar consola por defecto.**
  

    which zsh
    sudo usermod --shell /usr/bin/zsh usuario

**Para root**  

    usermod --shell /usr/bin/zsh root

  Reinicias **windows alt + q**

**Unir las dos configuraciones para no estar configurando.**

    sudo su
    cd ~/
    rm .zshrc
    ln -s -f /home/usuario/.zshrc .zshrc

**Instalar scrub**

    sudo apt-get install scrub -y

**Meterle lsd como dice el s4vitar**

Descargar [https://github.com/Peltoche/lsd/releases](https://github.com/Peltoche/lsd/releases)

El 0.14 [https://github.com/Peltoche/lsd/releases/download/0.14.0/lsd_0.14.0_amd64.deb](https://github.com/Peltoche/lsd/releases/download/0.14.0/lsd_0.14.0_amd64.deb)

  

    sudo su
    cd /home/usuario/Downloads
    dpkg -i lsd_0.14.0_amd64.deb

  
**instalar bat 0.12.1** [https://github.com/sharkdp/bat/releases](https://github.com/sharkdp/bat/releases)

[https://github.com/sharkdp/bat/releases/download/v0.12.1/bat_0.12.1_amd64.deb](https://github.com/sharkdp/bat/releases/download/v0.12.1/bat_0.12.1_amd64.deb)

  

    sudo su
    cd /opt/
    mv /home/usuario/Downloads/bat_0.12.1_amd64.deb
    dpkg -i bat_0.12.1_amd64.deb
    
 **Instalar bat extras** [https://github.com/eth-p/bat-extras](https://github.com/eth-p/bat-extras)


    sudo su
    cd /opt/
    git clone https://github.com/eth-p/bat-extras
    cd bat-extras
    ./build.sh

  

**Punto opcional si les funciona**

**instalar rg proceso de s4vitar**

    snap install rg

  Si no te sirve a la primera

    sudo su
    cd /opt
    git clone https://github.com/BurntSushi/ripgrep/releases/download/11.0.2/ripgrep_11.0.2_amd64.deb
    sudo dpkg -i ripgrep_11.0.2_amd64.deb

  

**Instalar fzf** https://github.com/junegunn/fzf

**Hacer ambos procedimientos en ambos usuarios.**

    cd ~
    git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
    ~/.fzf/install
    
**Instalar ranger**

    sudo su
    apt-get install ranger -y

**Instalar plugins**

    apt-get install zsh-autosuggestions -y
    apt-ge install zsh-syntaxhighlighting

  **Damos los mismos permisos de usuario.**

    sudo su
    cd /usr/share/
    chown usuario:usuario -R zsh-autosuggestions

**Instalar plugins manuales** https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/

**Ejemplo**

**Copiar link** [https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh](https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh)

  

    sudo su
    cd /usr/share
    mkdir zsh-sudo
    cd !$
    wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh
    chown thecyberpunk:thecyberpunk zsh-sudo -R
 

Instalar el bloqueo de sesión.

    sudo apt-get install i3lock-fancy imagemagick -y

**Por ultimo pegar toda la configuración como usuario normal.**

    nano ~/.zshrc
Pegar este código o el que comparte s4vitar que es el mismo, revisar primero las rutas que tenga nombre tus usuarios.

    # Fix the Java Problem
    export _JAVA_AWT_WM_NONREPARENTING=1
    
    # Enable Powerlevel10k instant prompt. Should stay at the top of ~/.zshrc.
    if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
      source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
    fi
    
    # Set up the prompt
    
    autoload -Uz promptinit
    promptinit
    prompt adam1
    
    setopt histignorealldups sharehistory
    
    # Use emacs keybindings even if our EDITOR is set to vi
    bindkey -e
    
    # Keep 1000 lines of history within the shell and save it to ~/.zsh_history:
    HISTSIZE=1000
    SAVEHIST=1000
    HISTFILE=~/.zsh_history
    
    # Use modern completion system
    autoload -Uz compinit
    compinit
    
    zstyle ':completion:*' auto-description 'specify: %d'
    zstyle ':completion:*' completer _expand _complete _correct _approximate
    zstyle ':completion:*' format 'Completing %d'
    zstyle ':completion:*' group-name ''
    zstyle ':completion:*' menu select=2
    eval "$(dircolors -b)"
    zstyle ':completion:*:default' list-colors ${(s.:.)LS_COLORS}
    zstyle ':completion:*' list-colors ''
    zstyle ':completion:*' list-prompt %SAt %p: Hit TAB for more, or the character to insert%s
    zstyle ':completion:*' matcher-list '' 'm:{a-z}={A-Z}' 'm:{a-zA-Z}={A-Za-z}' 'r:|[._-]=* r:|=* l:|=*'
    zstyle ':completion:*' menu select=long
    zstyle ':completion:*' select-prompt %SScrolling active: current selection at %p%s
    zstyle ':completion:*' use-compctl false
    zstyle ':completion:*' verbose true
    
    zstyle ':completion:*:*:kill:*:processes' list-colors '=(#b) #([0-9]#)*=0=01;31'
    zstyle ':completion:*:kill:*' command 'ps -u $USER -o pid,%cpu,tty,cputime,cmd'
    source /home/s4vitar/powerlevel10k/powerlevel10k.zsh-theme
    
    # To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
    [[ -f ~/.p10k.zsh ]] && source ~/.p10k.zsh
    
    # Manual configuration
    
    PATH=/root/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
    
    # Manual aliases
    alias ll='lsd -lh --group-dirs=first'
    alias la='lsd -a --group-dirs=first'
    alias l='lsd --group-dirs=first'
    alias lla='lsd -lha --group-dirs=first'
    alias ls='lsd --group-dirs=first'
    alias cat='bat'
    
    [ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
    
    # Plugins
    source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
    source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh
    source /usr/share/zsh-sudo/sudo.plugin.zsh
    
    # Functions
    function mkt(){
    	mkdir {nmap,content,exploits,scripts}
    }
    
    # Extract nmap information
    function extractPorts(){
    	ports="$(cat $1 | grep -oP '\d{1,5}/open' | awk '{print $1}' FS='/' | xargs | tr ' ' ',')"
    	ip_address="$(cat $1 | grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}' | sort -u | head -n 1)"
    	echo -e "\n[*] Extracting information...\n" > extractPorts.tmp
    	echo -e "\t[*] IP Address: $ip_address"  >> extractPorts.tmp
    	echo -e "\t[*] Open ports: $ports\n"  >> extractPorts.tmp
    	echo $ports | tr -d '\n' | xclip -sel clip
    	echo -e "[*] Ports copied to clipboard\n"  >> extractPorts.tmp
    	cat extractPorts.tmp; rm extractPorts.tmp
    }
    
    # Set 'man' colors
    function man() {
        env \
        LESS_TERMCAP_mb=$'\e[01;31m' \
        LESS_TERMCAP_md=$'\e[01;31m' \
        LESS_TERMCAP_me=$'\e[0m' \
        LESS_TERMCAP_se=$'\e[0m' \
        LESS_TERMCAP_so=$'\e[01;44;33m' \
        LESS_TERMCAP_ue=$'\e[0m' \
        LESS_TERMCAP_us=$'\e[01;32m' \
        man "$@"
    }
    
    # fzf improvement
    function fzf-lovely(){
    
    	if [ "$1" = "h" ]; then
    		fzf -m --reverse --preview-window down:20 --preview '[[ $(file --mime {}) =~ binary ]] &&
     	                echo {} is a binary file ||
    	                 (bat --style=numbers --color=always {} ||
    	                  highlight -O ansi -l {} ||
    	                  coderay {} ||
    	                  rougify {} ||
    	                  cat {}) 2> /dev/null | head -500'
    
    	else
    	        fzf -m --preview '[[ $(file --mime {}) =~ binary ]] &&
    	                         echo {} is a binary file ||
    	                         (bat --style=numbers --color=always {} ||
    	                          highlight -O ansi -l {} ||
    	                          coderay {} ||
    	                          rougify {} ||
    	                          cat {}) 2> /dev/null | head -500'
    	fi
    }
    
    function rmk(){
    	scrub -p dod $1
    	shred -zun 10 -v $1
    }
    
    # Finalize Powerlevel10k instant prompt. Should stay at the bottom of ~/.zshrc.
    (( ! ${+functions[p10k-instant-prompt-finalize]} )) || p10k-instant-prompt-finalize


Guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Reiniciamos y testeamos.**
