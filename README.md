Taiko Node Kurulum Rehberi
image

Taiko Alpha-3 Node
Merhaba taiko Alpha-3 Node kurulum rehberi
<< Hercules >>
🟢 Ön bilgi
Sistem 30303 port ile çalışıyor eğer 30303 portu başka bir uygulama kullanıyorsa altta port değiştirme işlemini yapın.

Taiko Bridge:
Bridge
Ağları Cüzdana Ekleme
Explorer
Linkler
Hercules Telegram
Hercules Twitter
Taiko Dc
🟢 Sistem özellikleri
Önerilern:

CPU - 16 cores
Memory - 32GB RAM
High-performance SSD with at least 1TB of free space
🟢 Sistem Güncelleme
sudo apt update
sudo apt upgrade
🟢 Docker Setup
apt install docker-compose
sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
🟢 1. Taiko dosyalarını indirin
git clone https://github.com/taikoxyz/simple-taiko-node.git
Screen Oluşturalım

screen -S taiko
taiko Klasörüne Giriş yapalım

cd simple-taiko-node
.env dosyası oluşturalım. Bu dosyayı oluşturduktan sonra Taiko platform testnetinde kullandığınız adresin Private keyini Tilki cüzdanınızdan alacaksınız ve .env dosyasına kaydedeceksiniz.

cp .env.sample .env
.Env dosyasına girelim burada değiştirmeniz gereken en alttaki bölüm.

nano .env



Şimdi sepolia RPC almamız gerekiyor Alchemy üzerinden alabilirsiniz.

image

L1_ENDPOINT_HTTP=Alchemy üzerinden alacağınız https linki
L1_ENDPOINT_WS=Alchemy üzerinden alacağınız wss linki

image


image




ENABLE_PROVER=true yapın
L1_PROVER_PRIVATE_KEY=Matemask adresinizin private keyini yazın

image

*ctrl + x Yes diyerek kaydediyoruz.



Private Key nasıl alınır sağdaki 3 noktaya tıklayın --- >> ardından hesap bilgileri --- >> Özel anahtarı dışa aktar

image


image

🟢 Çalıştırma
Bu işlem sonrası kurulum yapacak ve senkronize olmaya başlayacaktır.

docker compose up
image

🟢 Explorer üzerinden block görüntüleme
Explorer
🟢 Log Görme
Eğer başta screen oluşturmadıysanız bir screen oıluşturup logları görebilirsiniz.

cd simple-taiko-node
docker compose logs -f
🟢 Durdurma
Bu işlem sonrası bloklar silinmez sadece node durur.

docker compose down
🟢 Nodeyi silme
docker compose down -v
cd
rm -fr simple-taiko-node
🟢 Port çakışması yaşayanlar
.env dosyasındaki portları resimdeki gibi yapın

image

Artık sorunsuz çalıştırabilirsiniz.

🟢Token oluşturma
https://twitter.com/Hercules4413/status/1638791758635991040

🟢 Sözleşme Oluşturma ( Bunu yapmak şart değil isterseniz yapın )
Bu işlem sonrası kurulum yapacak ve senkronize olmaya başlayacaktır.

curl -L https://foundry.paradigm.xyz | bash
image

source /root/.bashrc
foundryup
image

forge init hello_foundry && cd hello_foundry
Nodeyi kurduğunuz cüzdanın private keyini yazın

forge create --legacy --rpc-url https://rpc.a2.taiko.xyz --private-key CÜZDAN-PRİVATE-KEY src/Counter.sol:Counter
image

Sözleşme aşağıdaki gibi oluştu. Kod bölümüne gelin ve doğrulayı tıklayın bir sayfa açılacak en altta doğrula butonuna basın

image

image

🟢 Sepolia Ağı TTKO token
Sepolia ağında TTKO test tokenleri geldiyse aşağıdaki işlemleri yapınız. Gelip gelmediğini kontrol etmek için isterseniz Tilki cüzdanınızdan explorerde görüntüle diyebilir yada tilki cüzdana aşağıdaki sözleşme adresini ekleyerek bakabilirsiniz.

TTKO Sözleşme adresi : 0xE52952B8063d0AE6Bd35E894866d8148976ce645

image

cd simple-taiko-node
docker compose down
Sözleşme üzerinden deposit işlemi yapın. 50 tane yatırmanız gerekiyor

https://sepolia.etherscan.io/address/0x6375394335f34848b850114b66a49d6f47f2cda8#writeProxyContract#F2

image

amount kısmına 5000000000 bunu yazın ve write butonuna basın ardından tilki cüzdanından onay isteyecek. Ardından aşağıdaki işlemlere devam edin.

nano .env
.env dosyanıza girdiğinizde en altta bulunan alanı aşağıdaki gibi değiştirin. CTRL + X YES komutu ile kaydedin ardından alttaki komut ile nodenizi çaıştırın.

ENABLE_PROPOSER=true
L1_PROPOSER_PRIVATE_KEY=MATEMASK CÜZDAN PRİVATE KEY
L2_SUGGESTED_FEE_RECIPIENT=MATEMASK CÜZDAN ADRESİNİZ

docker compose up
