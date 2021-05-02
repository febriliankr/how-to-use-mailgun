# Using Mailgun

install dependency 'mailgun-js' `npm i mailgun-js`

mailgunConfig.ts
```
import mailgun from "mailgun-js";
const mg = mailgun({
  apiKey: process.env.mailgunApiKey,
  domain: "mail.febrilian.com",
});

export default mg;

```

Serverless Function in /pages/api folder, eg: mail.ts

```
import { NextApiRequest, NextApiResponse } from "next";
import mg from "../../../config/mailgunConfig";

export default async (req: NextApiRequest, res: NextApiResponse) => {
  const { emailAddress } = req.body;

  const data = {
    from: "PLD UI 2022/2023 <noreply@pld-ui.com>",
    to: `${emailAddress}`,
    subject: `Pembayaran Anda Telah Diverifikasi`,
    text: `selamat`,
    html: `<p>Selamat</p>`,
  };

  mg.messages().send(data, function (error: any, body: any) {
    if (error) {
      res.json(error);
      return;
    }
    res.json(body);
  });
};
```
