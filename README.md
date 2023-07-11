# Template da aula de Async/Await

## Índice

-   [1. Prática 1](#1-prática-1)
-   [2. Prática 2](#2-prática-2)
-   [3. Prática 3](#3-prática-3)

## 1 Prática 1

### Enunciado

No template, altere a requisição getDadosUsuario para utilizar async/await juntamente com, try/catch

-   Modelo:

    ```
    const nomeDaConst = async () => {
            try {

            } catch (error) {

            }
    ```

### Resolução

-   Antes -> Com `then e catch`:

    ```
    const getDadosUsuario = () => {
            axios
                .get(`${BASE_URL}/${props.id}`, {
                    headers: {
                        Authorization: AUTH_TOKEN,
                    },
                })
                .then((res) => {
                    setUsuario(res.data);
                    setEmail(res.data.email);
                    setName(res.data.name);
                })
                .catch((err) => {
                    console.log(err.response);
                });
        };
    ```

-   Depois -> Com `try e catch`:

    ```
    const getDadosUsuario = async () => {
            try {
                const resp = await axios.get(`${BASE_URL}/${props.id}`, {
                    headers: {
                        Authorization: AUTH_TOKEN,
                    },
                });
                setUsuario(resp.data);
                setEmail(resp.data.email);
                setName(resp.data.name);
            } catch (error) {
                console.log(error);
            }
        };
    ```

-   Diferenças e Observações:
    -   Antes com `then e catch` eu fazia a requisição primeiro e depois usava o then e catch, usando o `try e catch` eu coloco a requisição dentro do `try`;
    -   Além disso preciso armazenar a resposta do `try` dentro de uma variável;


## 2 Prática 2

### Enunciado

Agora altere também a requisição `editaUsuario` para utilizar `async/await` juntamente `try/catch`.

### Resolução

-   Antes -> Com `then e catch`:

    ```
    const editaUsuario = () => {
            const body = {
                name,
                email,
            };
            axios
                .put(`${BASE_URL}/${usuario.id}`, body, {
                    headers: {
                        Authorization: AUTH_TOKEN,
                    },
                })
                .then(() => {
                    getDadosUsuario();
                    setEditar(!editar);
                });
        };
    ```

-   Depois -> Com `try e catch`:

    ```
    const editaUsuario = async () => {
            try {
                const body = {
                    name,
                    email,
                };
                await axios.put(`${BASE_URL}/${usuario.id}`, body, {
                    headers: {
                        Authorization: AUTH_TOKEN,
                    },
                });
                getDadosUsuario();
                setEditar(!editar);
            } catch (error) {
                console.log(error);
            }
        };
    ```

-   Diferenças e Observações:

-   Nesse caso, o `axios` não me retornar nenhum `dado`, ele só faz uma edição e armazena, por isso, eu não preciso de uma variável.

## 3 Prática 3

### Enunciado

Implemente a requisição de pesquisar usuários na função `pesquisaUsuario` usando a sintaxe `async/await` junto com `try/catch`.

### Resolução

-   Antes:

    ```
    const pesquisaUsuario = (pesquisa) => {};
    ```

-   Depois -> Com `try e catch`:

    ```
    const pesquisaUsuario = async (pesquisa) => {
        try {
            const resp = await axios.get(
                `${BASE_URL}/search?name=${pesquisa.nome}&email=${pesquisa.email}`,
                {
                    headers: {
                        Authorization: AUTH_TOKEN,
                    },
                }
            );
            resp.data.length ? setUsuarios(resp.data) : getUsuarios();
        } catch (error) {
            console.log(error.response);
        }
    };
    ```

    Nesse caso também é preciso um `useEffect` para acompanhar as mudanças e chamar a função quando for preciso:

    ```
    useEffect(() => {
        pesquisaUsuario(pesquisa);
    }, [pesquisa]);
    ```

-   Diferenças e Observações:

    -   Diferente da função da prática anterior, essa requisição vai retornar algo, então eu preciso armazenar os dados retornados em uma variável
