# Rádio dos Estudantes do ISCTE-IUL

Frontend da página de webstreaming da Rádio da AEISCTE-IUL. O server-side foi criado em PHP, acessando a base de dados para ir buscar as músicas a tocar e que passaram. (RadioDJ faz um request sempre que a música atualiza para inserir uma nova linha na base de dados com a música atual). As imagens dos artistas são obtidas através da API da Last.fm.

[live demo](http://www.aeiscte-iul.pt/radio/webstream/)

## To do
- Suporte para dispositivos móveis (elemento audio não funciona em telemóveis/tablets e media queries CSS).
- Finalização de pormenores da estilização da página (margens, min/max-width/height).
- (Server-side) Sincronizar a música a tocar com a stream. Atualmente tem um delay de ~15 segundos.
- Slider para volume
