#!/bin/bash
function pomoc() {
    echo "Wpisz Lab4.sh --opcja"
    echo "Dostępne opcje:"
    echo "  --help (-h)         wyświetlanie tego okna pomocy"
    echo "  --date (-d)         wyświetlanie aktualnej daty"
    echo "  --logs (-l)         tworzenie stu plików log.txt"
    echo "  --logs numer        tworzenie określonej ilości plików log"
    echo "  --init (i)          klonowanie repozytorium"
    echo "  --error numer (-e)  generowanie errorx.txt"
}

function pokaz_date() {
    aktualnaData=$(date +%d/%m/%Y)
    echo "Dzisiejsza data to: $aktualnaData"
}

function logi() {
    local liczba_plikow=${1:-100}
    for ((i=1; i<=liczba_plikow; i++)); do
        nazwaPliku="log$i.txt"
        echo "Nazwa Pliku: $nazwaPliku" > "$nazwaPliku"
        echo "Utworzone przez: $0" >> "$nazwaPliku"
        echo "Data utworzenia: $(date +%d/%m/%Y)" >> "$nazwaPliku"
    done
}
function init(){
    git clone https://github.com/maleckainez/IM-NarzedziaIT
        sciezkaPliku=$(dirname "($readlink -f "$0")")
    if [[ ":$PATH:" = *"$sciezkaPliku"*  ]]; then
        echo "Ścieżka jest w pliku PATH"
    else
        echo "Ścieżki nie ma w pliku PATH"
        export PATH="$PATH:sciezkaPliku"
        echo "Ale bez zmartwień, już została dodana"
    fi
    echo "Pomyślnie zainstalowano repozytorium"
}
function error(){
    local liczba_plikow=${1:-100}
    for ((i=1; i<=liczba_plikow; i++)); do
        sciezkaIplik="error$i"
        mkdir -p "$sciezkaIplik"
        touch "$sciezkaIplik/$sciezkaIplik.txt"
    done
}

case "$1" in
    -h|--help)
        pomoc
    ;;
    -d|--date)
        pokaz_date
    ;;
    -l|--logs)
        logi "$2"
    ;;
    -i|--init)
        init
    ;;
    -e|--error)
        error "$2"
    *)
        echo "Wybrano błędną opcję $1"
        pomoc
    ;;
esac
