# pos-printer


<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <title>Receipt example</title>
    <style>
        * {
            font-size: 12px;
            font-family: 'Times New Roman';
        }

        td,
        th,
        tr,
        table {
            border-top: 1px solid black;
            border-collapse: collapse;
        }

        td.description,
        th.description {
            width: 75px;
            max-width: 75px;
        }

        td.quantity,
        th.quantity {
            width: 40px;
            max-width: 40px;
            word-break: break-all;
        }

        td.price,
        th.price {
            width: 40px;
            max-width: 40px;
            word-break: break-all;
        }

        .centered {
            text-align: center;
            align-content: center;
        }

        .ticket {
            width: 155px;
            max-width: 155px;
        }

        img {
            max-width: inherit;
            width: inherit;
        }

        .leftAlign {
            text-align: left;
        }

        @media print {

            .hidden-print,
            .hidden-print * {
                display: none !important;
            }
        }
    </style>
</head>

<body>
    <div class="ticket">
        <!-- <img src="./logo.png" alt="Logo"> -->
        <p class="centered">ABCD PHARMACY
            <br>Dhaka-1216
        </p>
        <span>Name:{{$invoiceData->customer_name??''}}</span>
        <span>Date:{{$invoiceData->date??''}}</span>
        <table>
            <thead>
                <tr>
                    <th class="description leftAlign">Name</th>
                    <th class="quantity leftAlign">Qty</th>
                    <th class="price leftAlign">Price</th>
                    <th class="price leftAlign">Total</th>
                </tr>
            </thead>
            <tbody>
                @foreach($datas as $data)
                <tr>
                    <td class="description">{{$data->getProductInfo->name??''}}</td>
                    <td class="quantity">{{$data->product_quantity}}</td>
                    <td class="price">{{$data->product_purchase_price}}</td>
                    <td class="price">{{$data->product_quantity*$data->product_purchase_price}}</td>
                </tr>
                @endforeach
                <tr>

                    <td class="description">TOTAL = </td>
                    <td class="quantity"></td>
                    
                    <td class="price" colspan="2">{{$invoiceData->total_amount??''}} Tk</td>
                </tr>
                @if($invoiceData->discount_amount>0)
                <tr>
                    <td class="description">Discount = </td>
                    <td class="quantity"></td>
                    
                    <td class="price" colspan="2">-{{$invoiceData->discount_amount}} Tk</td>
                </tr>
                @endif

                @if($invoiceData->paid_amount>0)
                <tr>
                    <td class="description">Payment = </td>
                    <td class="quantity"></td>
                    <td class="price" colspan="2">{{$invoiceData->paid_amount}}</td>
                </tr>
                @endif

                <tr>
                    <td class="description">Due = </td>
                    <td class="quantity"></td>
                    <td class="price" colspan="2">{{$invoiceData->total_amount- ($invoiceData->paid_amount+$invoiceData->discount_amount)}} Tk</td>
                </tr>


            </tbody>
        </table>
        <p class="centered">Developed By
            <br>www.digitalwebexpert.com
            <br>017XXXXXXXXX
        </p>
    </div>
    <button id="btnPrint" class="hidden-print">Print</button>
</body>
<script>
    window.onload = function() {
        window.print();
    }
    const $btnPrint = document.querySelector("#btnPrint");
    $btnPrint.addEventListener("click", () => {
        window.print();
    });
</script>

</html>
