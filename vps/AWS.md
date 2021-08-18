
```sh
# o primeiro comando copia arquivos de um lado pra outro (pode usar *)
aws s3 cp <arquivos locais> s3://<nome do bucket>/<nome da pasta - se existir>

# faz um sync da pasta inteira, da origem para o destino de dentro da instancia
aws s3 sync <pasta local> s3://<nome do bucket>/<nome da pasta - se existir>


# criar as invalidações do cloudfront, você pode fazer pelo console web ou dá pra usar o aws cli tb
aws cloudfront create-invalidation --distribution-id <ID da distribuição do cloudfront> --paths <lista, separada por espaços, dos paths que serão invalidados>


# exemplo do comando do cloudfront: 
aws cloudfront create-invalidation --distribution-id EDFDVBD6EXAMPLE --paths "/example-path/example-file.jpg" "/example-path/example-file2.png"
```