# Mixin para gerar font-faces de fontes commitadas no projeto

Usado para evitar a chamada externa de fontes, tornando a requisição mais rápida e confiável. 

## Como obter as fontes?

Para baixar as fontes do [Google Fonts](https://fonts.google.com) por exemplo, siga os passos abaixo. Você também pode usar fontes de autoria própria ou fazer o download de outros locais.

- Ao acessar o [Google Fonts](https://fonts.google.com), localize a fonte desejada na busca, e clique no ícone **+**.

![1-opensans-add.png](https://bitbucket.org/repo/R9nageM/images/3143653494-1-opensans-add.png)

- Ao adicionar a fonte, localize um painel que estará minimizado no rodapé da página. Clique sobre ele para maximizá-lo.

![2-opensans-customize.png](https://bitbucket.org/repo/R9nageM/images/2634444051-2-opensans-customize.png)

- Clique na aba **customize**, e selecione os pesos e estilos que deseja baixar. Clique sobre o ícone de download no canto superior direito do painel.

![3-opensans-add-weights.png](https://bitbucket.org/repo/R9nageM/images/1171848341-3-opensans-add-weights.png)

- Extraia o zip baixado, no diretório onde as fontes ficarão no projeto, lembrando de excluir os arquivos que não serão usados, como txts, htmls, etc. Mantenha apenas os arquivos que contem extensão de fontes (eot, svg, ttf, woff, woff2). 

- **DICA: Caso esteja usando uma fonte que não forneça os 5 formatos usados para compatibilidade entre navegadores, como é o caso do Google Fonts, você pode gerá-los a partir dos TTFs [aqui](https://www.fontsquirrel.com/tools/webfont-generator).**.

## Otimização de requisições

Para cada peso e estilo de cada fonte é gerado um @font-face, e para cada @font-face, são feitas pelo menos 5 requisições para os diferentes formatos, para compatibilidade entre navegadores e versões (eot, svg, ttf, woff, woff2). **Sendo assim, adicione ao projeto somente as fontes, pesos e estilos descritas em seu style guide, evitando requisições desnecessárias que podem deixar o carregamento mais lento.**

Se em meu layout tenho apenas Montserrat (100, 500) e Open Sans (100, 300, 400, 500), é recomendado chamar apenas estas, evitando pesos e estilos que não são usados e requisições desnecessárias. 

O carregamento de fontes pode ser parte da causa de sites pouco performáticos, não importando se as requisições são feitas de dentro do projeto ou externas, como o Google Fonts. Se houverem chamadas desnecessárias em qualquer um dos casos, a performance pode ser prejudicada.

## Como usar?

- Inclua o arquivo **_generate_font_face_mixin.sass** em seu projeto, junto com as demais partials do SASS. Faça o import da partial no arquivo principal, onde todas as partials estão sendo importadas.

```
...
@import "generate_font_face_mixin"
...
```

- Renomeie os arquivos de fontes, seguindo o padrão **<prefixo>-<peso><estilo>.<formato>**, em lowercase. Veja o exemplo abaixo para a fonte **Open Sans**:

    - opensans-bold.eot
    - opensans-bold.svg
    - opensans-bold.ttf
    - opensans-bold.woff
    - opensans-bold.woff2
    - opensans-bolditalic.eot
    - opensans-bolditalic.svg
    - opensans-bolditalic.ttf
    - opensans-bolditalic.woff
    - opensans-bolditalic.woff2
    ...

    ***OBS.:*** *Após a padronização e geração de todos os formatos para cada peso e estilo, o diretório terá pelo menos 5 arquivos para cada peso e estilo com formatos diferentes (eot, svg, ttf, woff, woff2), com objetivo de ser compatível com diferentes navegadores em diferentes versões.*

- Faça a chamada do mixin onde serão declarados os padrões de fontes, informando os parâmetros:

    - **Caminho da fonte**, relativo ao local de seu arquivo CSS após a compilação. Exemplo: se o seu arquivo CSS está em **assets/dist**, e suas fontes estão em **assets/fonts**, então seu path será **../fonts**, e se houver divisão de fontes dentro deste, ficará **../fonts/<diretorio-da-fonte>**.

    - **Nome da fonte**.

    - **Prefixo dos arquivos**, já padronizados.

    - **Estilo da fonte**, para fontes com italic, a ser inserido em **font-style**. Se desejar incluir uma fonte com todos os pesos (font-weight), e que também contém italic, duas chamadas para o mixin precisam ser feitas. Veja o exemplo abaixo com a fonte *Montserrat*.

    - **Pesos das fontes**, a serem iterados e inseridos em **font-weight** para cada font-face.
        ***OBS.:*** *Os pesos citados acima são equivalentes as versões da mesma fonte: 100 ('thin'), 200 ('extralight') 300 ('light'), 400 ('regular'), 500 ('medium'), 600 ('semibold'), 700 ('bold'), 800 ('extrabold'), 900 ('black'). Inclua somente os que estão presentes no diretório indicado no primeiro parâmetro para a fonte especificada.*

##### Exemplo de chamada do mixin para Open Sans e Open Sans Italic com os pesos thin, light, regular, medium e semibold.

```
+generateFontFaces('../fonts/opensans', 'Open Sans', 'opensans', '', (100, 300, 400, 500, 600))
+generateFontFaces('../fonts/opensans', 'Open Sans', 'opensans', italic, (100, 300, 400, 500, 600))
```


##### Resultado do generateFontFaces após a compilação do SASS

```
@font-face {
    font-family:"Open Sans";
    font-weight:300;
    font-style:normal;src:url("../fonts/opensans/opensans-light.eot");src:url("../fonts/opensans/opensans-light.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-light.woff2") format("woff2"),url("../fonts/opensans/opensans-light.woff") format("woff"),url("../fonts/opensans/opensans-light.ttf") format("truetype"),url("../fonts/opensans/opensans-light.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:400;
    font-style:normal;src:url("../fonts/opensans/opensans-regular.eot");src:url("../fonts/opensans/opensans-regular.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-regular.woff2") format("woff2"),url("../fonts/opensans/opensans-regular.woff") format("woff"),url("../fonts/opensans/opensans-regular.ttf") format("truetype"),url("../fonts/opensans/opensans-regular.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:600;
    font-style:normal;src:url("../fonts/opensans/opensans-semibold.eot");src:url("../fonts/opensans/opensans-semibold.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-semibold.woff2") format("woff2"),url("../fonts/opensans/opensans-semibold.woff") format("woff"),url("../fonts/opensans/opensans-semibold.ttf") format("truetype"),url("../fonts/opensans/opensans-semibold.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:700;
    font-style:normal;src:url("../fonts/opensans/opensans-bold.eot");src:url("../fonts/opensans/opensans-bold.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-bold.woff2") format("woff2"),url("../fonts/opensans/opensans-bold.woff") format("woff"),url("../fonts/opensans/opensans-bold.ttf") format("truetype"),url("../fonts/opensans/opensans-bold.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:800;
    font-style:normal;src:url("../fonts/opensans/opensans-extrabold.eot");src:url("../fonts/opensans/opensans-extrabold.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-extrabold.woff2") format("woff2"),url("../fonts/opensans/opensans-extrabold.woff") format("woff"),url("../fonts/opensans/opensans-extrabold.ttf") format("truetype"),url("../fonts/opensans/opensans-extrabold.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:300;
    font-style:italic;src:url("../fonts/opensans/opensans-lightitalic.eot");src:url("../fonts/opensans/opensans-lightitalic.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-lightitalic.woff2") format("woff2"),url("../fonts/opensans/opensans-lightitalic.woff") format("woff"),url("../fonts/opensans/opensans-lightitalic.ttf") format("truetype"),url("../fonts/opensans/opensans-lightitalic.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:400;
    font-style:italic;src:url("../fonts/opensans/opensans-regularitalic.eot");src:url("../fonts/opensans/opensans-regularitalic.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-regularitalic.woff2") format("woff2"),url("../fonts/opensans/opensans-regularitalic.woff") format("woff"),url("../fonts/opensans/opensans-regularitalic.ttf") format("truetype"),url("../fonts/opensans/opensans-regularitalic.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:600;
    font-style:italic;src:url("../fonts/opensans/opensans-semibolditalic.eot");src:url("../fonts/opensans/opensans-semibolditalic.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-semibolditalic.woff2") format("woff2"),url("../fonts/opensans/opensans-semibolditalic.woff") format("woff"),url("../fonts/opensans/opensans-semibolditalic.ttf") format("truetype"),url("../fonts/opensans/opensans-semibolditalic.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:700;
    font-style:italic;src:url("../fonts/opensans/opensans-bolditalic.eot");src:url("../fonts/opensans/opensans-bolditalic.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-bolditalic.woff2") format("woff2"),url("../fonts/opensans/opensans-bolditalic.woff") format("woff"),url("../fonts/opensans/opensans-bolditalic.ttf") format("truetype"),url("../fonts/opensans/opensans-bolditalic.svg#Open Sans") format("svg")
}

@font-face {
    font-family:"Open Sans";
    font-weight:800;
    font-style:italic;src:url("../fonts/opensans/opensans-extrabolditalic.eot");src:url("../fonts/opensans/opensans-extrabolditalic.eot?#iefix") format("embedded-opentype"),url("../fonts/opensans/opensans-extrabolditalic.woff2") format("woff2"),url("../fonts/opensans/opensans-extrabolditalic.woff") format("woff"),url("../fonts/opensans/opensans-extrabolditalic.ttf") format("truetype"),url("../fonts/opensans/opensans-extrabolditalic.svg#Open Sans") format("svg")
}
```

##### Usando a fonte

Após a padronização dos arquivos de fontes e a geração dos @font-faces, utilize a fonte conforme o exemplo abaixo.

```
h1
    font-family: 'Open Sans', sans-serif
    font-weight: 600
    ...
```

**Versão itálica:**

```
h1
    font-family: 'Open Sans', sans-serif
    font-weight: 600
    font-style: italic
    ...
```