# NextJS with Prisma

This sample project shows how to use NextJS with Prisma.

Prisma is an open-source ORM and database toolkit that makes working with
databases a breeze. You can learn more at [prisma.io](http://prisma.io).

## How to Run the Project

Clone the repo and install dependencies.

```bash
# with npm
npm i

# with yarn
yarn
```

After all dependencies have been installed, run the app.

```bash
# with npm
npm run dev

# with yarn
yarn dev
```

The app will be served at `localhost:3000`.

This sample project demonstrates how to access data with Prisma in two distinct
ways: in the `getServerSideProps` function and from an API route.

## Access from `getServerSideProps`

The `getServerSideProps` function is used to prepare a NextJS page before it
renders. It's useful for getting some initial data to be rendered.

This function runs on the server which means Prisma can be used here to query
for data.

The project demonstrates how to make an initial query for data with Prisma in
this function.

```ts
// pages/index.ts

export async function getServerSideProps() {
  const contacts = await prisma.contact.findMany();
  return {
    props: {
      initialContacts: contacts
    }
  };
}
```

## Access from an API Route

Next API routes use serverless functions to execute code in a backend
environment. This is another place where Prisma can be used to access data.

This project demonstrates how to save data with Prisma from an API route.

```ts
// pages/api/contacts.ts

import type { NextApiRequest, NextApiResponse } from 'next';

import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

export default async (req: NextApiRequest, res: NextApiResponse) => {
  if (req.method !== 'POST') {
    return res.status(405).json({ message: 'Method not allowed' });
  }

  try {
    const contact = JSON.parse(req.body);
    const savedContact = await prisma.contact.create({ data: contact });
    res.status(200).json(savedContact);
  } catch (err) {
    res.status(400).json({ message: 'Something went wrong' });
  }
};
```

## License

MIT
