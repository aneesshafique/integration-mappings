This project asks you to implement an endpoint that exposes different views or mappings on an underlying dataset. Our team is primarily focused on two types of work:
* converting data between an insurance carrier's or broker's representation and ThreeFlow's
* writing an API to expose our (current!) best way of thinking about that data.

In this project, you'll do both things:
* you'll write an endpoint to an OAI spec (you can copy-paste this into [a Swagger editor](https://editor-next.swagger.io/) for a nice view)
* you'll shape the same data in two very different ways for different users.

## The ask

We ask that you take no more than 2 hours on this project. The point is not for it to be 'done', but for it to show how you think about this type of work and provide a springboard for a conversation in our interview.

Below you'll find data mappings for the dental and vision coverages for two insurance carriers. They think about all the data points fairly differently (as they do in the real world!). There is one endpoint defined in the app (if you run the server and hit http://localhost:3000/opportunities/2?carrier_id=1 you should see some data!). We want that app to be able to:
* Represent each known product as either carrier `1` or carrier `2`.
* In each representation, match the spec defined in `doc/api.yml`.

The mappings likely have gaps, so you'll have to make assumptions. Document those in the README you share back with us.

## Assumptions/Questions

< Your notes here! >
# Data Mappings

## Acme

### Dental

| Their Field | Our Field         | Mapping Logic                                                                                                                           |
| ----------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| id          | id                |                                                                                                                                         |
| commissions | commissions       | If our number is a percentage, render as decimal with 2 significant digits. <br/>Otherwise leave `nil`                                  |
| xray        | x_ray_coinsurance | If our number is a percentage, render as a whole number percentage (string). Round up to get to whole number.<br/>Otherwise leave `nil` |

### Vision

| Their Field        | Our Field   | Mapping Logic                                                                                                                                                                                 |
| ------------------ | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                 | id          |                                                                                                                                                                                               |
| broker_commissions | commissions | If number is a percent, round down to appropriate enum.<br/>If number is 'PEPM' or 'Per Employee' or 'Per Employee Per Month' (case insensitive), use `PerEmployee`<br/>Otherwise leave `nil` |
| frame_benefit      | frames      | If a dollar amount, round down to whole number.<br/>Otherwise leave `nil`                                                                                                                     |

## Demo Carrier
### Dental

| Their Field                       | Our Field         | Mapping Logic                                                                                    |
| --------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------ |
| id                                | id                |                                                                                                  |
| commissions>commissions_pct       | commissions       | If our number is a percentage, render as whole number. Round down. <br/>Otherwise leave `nil`    |
| commissions>commissions_structure | commissions       | If number is a percentage, `Percent`.<br/>If number is 'flat', `Flat`.<br/>Otherwise leave `nil` |
| x_ray_coinsurance                 | x_ray_coinsurance | If our number is a percentage, round up to correct enum.<br/>Otherwise leave `nil`               |

### Vision

| Their Field        | Our Field   | Mapping Logic                                                                       |
| ------------------ | ----------- | ----------------------------------------------------------------------------------- |
| id                 | id          |                                                                                     |
| broker_commissions | commissions | If number is a percent, display with 2 significant digits<br/>Otherwise leave `nil` |
| frame_benefit      | frames      | If a dollar amount, round up to matching enum.<br/>Otherwise leave `nil`            |

