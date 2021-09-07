# mensagem-leve

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

Para chamar a API para buscar as conversas:
https://api.telegram.org/bot + token +/getupdates

Para chamar a API para buscar as conversas:
const target = "https://api.telegram.org/bot" + token + "/sendMessage?chat_id=" + idChat + "&text=" + mensagem;