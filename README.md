# s4vitarSetup
Instalando el entorno de @S4vitar paso a paso.

# s4vitar configuracion

Actualizar parrot .

apt update

**Ejecutar los siguientes comandos en usuario root**
 
Instalar Bpswm.

`apt-get install bspwm -y` 
 Instalar dependencias que encuentras aqui.  [https://github.com/baskerville/bspwm/wiki](https://github.com/baskerville/bspwm/wiki "https://github.com/baskerville/bspwm/wiki")

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
 **Pegamos el siguiente codigo** 
 
  `sxhkd & exec bspwm`

guardamos **ctrl + o** [enter] y luego **ctrl + x**.

**Instalamos compton (transparencia) , feh (fondo de pantalla) y rofi (lanzador de aplicaciones)** 

    sudo apt-get install compton -y
    sudo apt-get install feh -y
    sudo apt-get install rofi -y


Los archivos de configuracion que se usaran en esta guia los puedes encontrar en  [https://s4vitar.github.io/bspwm-configuration-files/#](https://s4vitar.github.io/bspwm-configuration-files/#).


**Nota.**
Modificas los nombres de usuario tambien creas una carpeta en el escritorio o le das la ruta a la **linea feh** para especificar donde se encuentra tu fondo de pantalla o si quieres solo aniades las lineas faltantes. 
tambien tener en cuenta que en la linea **pointer_modifier mod1** puede variar por mod2 mod3 mod4 depende la configuracion de tu escritorio.

    cd ~/.config/bspwm
    nano bspwmrc

**Borras el codigo y pegar este en el archivo bspwmrc**

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

**Copiar el archivo de configuracion sxhkdrc de s4vitar**

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

**pegamos la configuracion de s4vitar en el archivo bspwm_resize**

    nano bspwm_resize
pegar

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

**Reiniciar sesion**

    kill -9 -1
   
  iniciamos sesion con **bspwm**, lo escojemos en la parte superior del login.

 
Una vez se inicie sesion, no se vera nada, para ella damos **windows + enter** de esta manera se abrira una terminal.


**Crear el archivo de configuracion para transparencia con compton**

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
