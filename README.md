Шаблон адаптера переводить один інтерфейс (властивості та методи об'єкта) в інший. Адаптери дозволяють програмним компонентам працювати разом, що в іншому випадку було б &lstqup;t через невідповідність інтерфейсів. Візерунок адаптера також називають візерунком обгортки.

Один із сценаріїв, у якому зазвичай використовуються адаптери, – це коли потрібно інтегрувати нові компоненти та працювати разом з наявними компонентами в програмі.

Інший варіант розвитку подій - рефакторінг, при якому частини програми переписуються з поліпшеним інтерфейсом, але старий код все одно очікує оригінальний інтерфейс.

![image](https://user-images.githubusercontent.com/46648541/227902217-a7e623fa-2c28-42f9-a3ee-c0dcd0c23a76.png)


Учасники
#
Об'єктами, що беруть участь в цій закономірності, є:

Клієнт -- У прикладі коду: функція run().
виклики в адаптер для запиту послуги

Адаптер -- У прикладі коду: ShippingAdapter
реалізує інтерфейс, який очікує або знає клієнт

Adaptee -- У прикладі коду: Розширена доставка
Об'єкт, що адаптується
має інтерфейс, відмінний від того, що очікує або знає клієнт

Приклад коду нижче показує онлайн-кошик для покупок, в якому об'єкт доставки використовується для обчислення вартості доставки. Старий об'єкт доставки замінюється новим і вдосконаленим об'єктом доставки, який є більш безпечним і пропонує кращі ціни.

Новий об'єкт отримав назву AdvancedShipping і має зовсім інший інтерфейс, якого клієнтська програма не очікує. ShippingAdapter дозволяє клієнтській програмі продовжувати функціонувати без будь-яких змін API, зіставляючи (адаптуючи) старий інтерфейс доставки до нового інтерфейсу AdvancedShipping.


// old interface

function Shipping() {
    this.request = function (zipStart, zipEnd, weight) {
        // ...
        return "$49.75";
    }
}

// new interface

function AdvancedShipping() {
    this.login = function (credentials) { /* ... */ };
    this.setStart = function (start) { /* ... */ };
    this.setDestination = function (destination) { /* ... */ };
    this.calculate = function (weight) { return "$39.50"; };
}

// adapter interface

function ShippingAdapter(credentials) {
    var shipping = new AdvancedShipping();

    shipping.login(credentials);

    return {
        request: function (zipStart, zipEnd, weight) {
            shipping.setStart(zipStart);
            shipping.setDestination(zipEnd);
            return shipping.calculate(weight);
        }
    };
}

function run() {

    var shipping = new Shipping();
    var credentials = { token: "30a8-6ee1" };
    var adapter = new ShippingAdapter(credentials);

    // original shipping object and interface

    var cost = shipping.request("78701", "10010", "2 lbs");
    console.log("Old cost: " + cost);

    // new shipping object with adapted interface

    cost = adapter.request("78701", "10010", "2 lbs");

    console.log("New cost: " + cost);
}
