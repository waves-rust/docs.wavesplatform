# Основные понятия

**Блокчейн** — технология децентрализованного хранения данных, обеспечивающая их неизменяемость и прозрачность.

## Транзакции и блоки

Данные на блокчейне представлены в виде транзакций — записей о действиях, таких как выпуск или перевод криптовалюты, создание или вызов смарт-контракта и т. д.

Транзакции сгруппированы в блоки. Кроме транзакций, блок содержит хеш данных предыдущего блока и цифровую подпись ноды, которая сгенерировала блок. Предыдущий блок, в свою очередь, содержит хеш данных предшествующего ему блока, и т. д. Получается, что подпись каждого блока зависит от данных всех предыдущих блоков.

Таким образом, блокчейн состоит из цепочки блоков, связанных с помощью криптографии. Каждая транзакция навсегда остается на блокчейне в первозданном виде: изменение данных в блоке сделало бы невалидным и этот блок, и все последующие блоки.

![](./_assets/blockchain.png)

## Нода

Нода — это компьютер, обслуживающий сеть блокчейна. Нода Waves хранит полную копию блокчейна, проверяет валидность всех транзакций и блоков, правильность подписей и хешей и синхронизирует данные с другими нодами.

Сеть Waves состоит из сотен нод по всему миру. Благодаря этому данные блокчейна защищены от подделки и от удаления. [Запустить свою ноду](/ru/waves-node/how-to-install-a-node/how-to-install-a-node) и присоединиться к сети может любой желающий.

![](./_assets/nodes-worldwide.png)

Ноды могут участвовать в генерации новых блоков и получать [вознаграждение за создание блока](/ru/blockchain/mining/mining-reward) и комиссии за транзакции. Чтобы начать генерировать блоки, необходим баланс не менее 1000 WAVES — собственных или [полученных в лизинг](/ru/blockchain/leasing). Чем больше генерирующий баланс ноды, тем больше шанс получить право на генерацию следующего блока.

## Аккаунт

Waves использует модель на основе аккаунта. Каждая транзакция создается от имени какого-либо аккаунта, все активы и данные связаны с аккаунтом. У аккаунта есть пара ключей, связанных с помощью криптографии: закрытый ключ, которым аккаунт подписывает транзакции, и открытый ключ, который позволяет кому угодно проверить достоверность подписи. [Подробнее об аккаунте](/ru/blockchain/account/)

Для создания аккаунта, хранения ключей и подписания транзакций можно использовать приложения: [Waves.Exchange](https://waves.exchange/), разработанное командой Waves.Exchange; [WavesFX](https://wavesfx.github.io/), разработанное командой WavesFX; [Waves Keeper](/ru/ecosystem/waves-keeper/), разработанное командой Waves; и другие.

![](./_assets/interaction.png)

## dApp

dApp (decentralized application) — это приложение, бизнес-логика которого работает на блокчейне. Приложение может хранить на блокчейне данные, а также вызывать смарт-контракт — скрипт, прикрепленный к аккаунту. Преимущество децентрализованных приложений — в надежности хранения данных и прозрачности: любой пользователь может посмотреть код скрипта и результат его вызова.

Чтобы узнать больше о технологиях Waves и получить практические навыки создания децентрализованных приложений, присоединяйтесь к бесплатному курсу [Mastering Web3 with Waves](https://www.coursera.org/learn/mastering-web3-waves) на платформе Coursera.
