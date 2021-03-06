# .NET Console

Repositório contendo alguns exemplos práticos demonstrando como converter uma imagem para Base64 ou como salvar uma imagem que esta em Base64 em um diretório do seu computador.


### Classe Helper com os métodos estáticos


```Csharp
class Helper
    {
        /// <summary>
        /// Download de imagem da internet para convertendo em base64
        /// </summary>
        /// <returns></returns>
        public static string ImagemDaInternetParaBase64()
        {
            string path = Directory.GetCurrentDirectory() + @"\Imagem\marvel.jpg";
            var client = new WebClient();
            client.DownloadFile("https://miro.medium.com/proxy/0*BPmJlsN9r5OgAX1r.jpg", path);

            using (Image image = Image.FromFile(path))
            {
                using (MemoryStream m = new MemoryStream())
                {
                    image.Save(m, image.RawFormat);
                    byte[] imageBytes = m.ToArray();
                    var base64String = Convert.ToBase64String(imageBytes);
                    return base64String;
                }
            }
        }


        /// <summary>
        /// Convertendo imagem em disco para base64
        /// </summary>
        /// <returns></returns>
        public static string ImagemParaBase64()
        {
            string path = Directory.GetCurrentDirectory() + @"\Imagem\paisagem.jpg";

            using (Image image = Image.FromFile(path))
            {
                using (MemoryStream m = new MemoryStream())
                {
                    image.Save(m, image.RawFormat);
                    byte[] imageBytes = m.ToArray();
                    var base64String = Convert.ToBase64String(imageBytes);
                    return base64String;
                }
            }
        }

        /// <summary>
        /// Recebendo uma imagem em base64, convertendo ela em um arquivo de imagem em memoria e salvando em disco
        /// </summary>
        /// <param name="base64String"></param>
        public static void Base64ParaImagem(string base64String)
        {

            var imageBytes = Convert.FromBase64String(base64String);
            using (var ms = new MemoryStream(imageBytes, 0, imageBytes.Length))
            {
                var image = Image.FromStream(ms, true);
                var fileName = Guid.NewGuid() + ".png";
                string path = Directory.GetCurrentDirectory() + @"\Imagem\" + fileName;
                image.Save(path, ImageFormat.Png);
            }
        }
    }
```

