#!/bin/bash

# cbr2cbz
# Convert cbr files to cbz.
#
# pixatintes@gmail.com 
#
#		04.09.2022 : v0.1	
# Install
# 		Put on ~/.gnome2/nautilus-scripts/ or ~/.local/share/nautilus/scripts/
#		In a console : chmod +x cdr2cbz
# Dependency
#		unrar
#		unzip
#		ImageMagick
#		pdftk
#		
version="0.1"
#########################################################################
#!/bin/bash

	###### Default = English #####
	title="cbr2cbz "$version""
	pleasesel="Please select a file."
	noselec=""$title" convert cbr/cbz files. "$pleasesel""
	select="Select a format:"
	Standard="Convert only" 
	Delete="Convert and remove the original cbr files (Dangerous)"
	warning="Warning"
	proceed="is already exist. Overwrite?"
	conversion="Converting files.."
	end="Done :P"

case $LANG in
	######## Spanish ########
	ca* )
	title="cbr2cbz "$version""
	pleasesel="Sel·leccina almenys un arxiu."
	noselec=""$title" converteix cbr/cbz arxius. "$pleasesel""
	select="Sel·lecciona un format:"
	Standard="Conversió estàndard" 
	Delete="Converteix i esborra els originals (Perillós)"
	warning="Compte!"
	proceed="ja existeix. Sobreescriure?"
	conversion="Convertint arxius.."
	end="Feina feta :P"
esac

#################################################
#	FUNCIONS


ext() # funció "convert file"
{
	#### Extreure cbr/cbz arxius a ~/tempcbr/

	O=`echo "$1" | sed 's/\.\w*$/''/'`	

	if [ "`file -b "$1" | grep 'RAR'`" != 0 ]
	then
		mkdir .tempcbr
		unrar e -y "$1" .tempcbr/
	fi

	if [ "`file -b "$1" | grep 'Zip'`" != 0 ]
	then
		unzip "$1" -d .tempcbr/
	fi


	#### Crear pdf

	if [ "$2" = "Standard" ]
	then # Standard
		zip -r temp.cbz .tempcbr 
	fi

	if [ "$2" = "Delete" ]
	then # Standard
		zip -r temp.cbz .tempcbr 
		rm "$1"
	fi


	##### borrar temp directori ~/tempcbr/
	rm -R .tempcbr
}

IFS=$'\t\n'

#### No hi ha fitxers sel. ###
			
if [ $# -eq 0 ]; then
	zenity --error --title="$warning" --text="$noselec"
	exit 1
fi		

######## Finetra principal ########
while [ ! "$formatout" ] # Preguntar el format de sortida
do	
	formatout=`zenity --list --title="$title" --text="$select" --width=680 --height=270 --column="Format" --column="Description" Standard "$Standard" Delete "$Delete"`

	[ $? -ne 0 ] && exit 2 # Cancelar
done

(while [ $# -gt 0 ]; do
	for i in $formatout; do
		O=`echo "$1" | sed 's/\.\w*$/'.pdf'/'`			
		while `true`; do
			########## Mirar si el fitxer existeix, sobreescriu ? ##########
			if [ "`ls "$O" | grep -v "^ls"`" != "" ]
			then
				if !(`gdialog --title "$warning" --yesno "$O $proceed" 200 100`)
				then
					break
				fi
			fi
			ext "$1" "$formatout" # Convertir
			
			mv temp.cbz "$O".cbz

		break
		shift
		done
	done
	shift
done


echo "# "$end"" ; sleep 1
) |
#### Barra de progrés ####
zenity --progress --percentage=0 --title="$title" --text="$conversion" --pulsate --width=400 

