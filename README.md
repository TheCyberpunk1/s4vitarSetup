# s4vitarSetup
Instalando el entorno de @S4vitar paso a paso.

# s4vitar configuracion
Actualizar parrot .

```c
apt update
```

**Ejecutar los siguientes comandos en usuario root
**
Instalar Bpswm.

` apt-get install bspwm -y `
Instalar dependencias que encuentras aqui. [https://github.com/baskerville/bspwm/wiki](https://github.com/baskerville/bspwm/wiki "https://github.com/baskerville/bspwm/wiki")
 
`sudo apt-get install libxcb-xinerama0-dev libxcb-icccm4-dev libxcb-randr0-dev libxcb-util0-dev libxcb-ewmh-dev libxcb-keysyms1-dev libxcb-shape0-dev`

**Ahora como usuario normal.**
"git clone https://github.com/baskerville/bspwm.git

git clone https://github.com/baskerville/sxhkd.git

cd bspwm && make && sudo make install

cd ../sxhkd && make && sudo make install

mkdir -p ~/.config/{bspwm,sxhkd}

cp /usr/local/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/

cp /usr/local/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/

chmod u+x ~/.config/bspwm/bspwmrc"

**Editamos el xinitrc para que arranque el escritorio de bspmw.**

`nano ~/.xinitrc
`
**Pegamos el siguiente codigo
**
`sxhkd & 

exec bspwm`

guardamos ctrl + o [enter] y luego ctrl + x.





