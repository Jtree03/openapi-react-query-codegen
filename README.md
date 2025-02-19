# OpenAPI React Query Codegen

> Node.js library that generates [React Query (also called TanStack Query)](https://tanstack.com/query) hooks based on an OpenAPI specification file.

## Features

- Supports generation of custom react hooks that use React Query's `useQuery` and `useMutation` hooks
- Supports generation of query keys for query caching
- Supports the option to use pure TypeScript clients generated by [OpenAPI Typescript Codegen](https://github.com/ferdikoomen/openapi-typescript-codegen)

## Install

```
$ npm install -D @7nohe/openapi-react-query-codegen
```

## Usage

```
$ openapi-rq --help

Usage: openapi-rq [options]

Generate React Query code based on OpenAPI

Options:
  -V, --version              output the version number
  -i, --input <value>        OpenAPI specification, can be a path, url or string content (required)
  -o, --output <value>       Output directory (default: "openapi")
  -c, --client <value>       HTTP client to generate [fetch, xhr, node, axios, angular] (default: "fetch")
  --useUnionTypes            Use union types (default: false)
  --exportSchemas <value>    Write schemas to disk (default: false)
  --indent <value>           Indentation options [4, 2, tabs] (default: "4")
  --postfixServices <value>  Service name postfix (default: "Service")
  --postfixModels <value>    Modal name postfix
  --request <value>          Path to custom request file
  -h, --help                 display help for command
```

## Example Usage

### Command

```
$ openapi-rq -i ./petstore.yaml
```

### Output directory structure

```
- openapi
  - queries
    - index.ts <- custom react hooks
  - requests <- output code generated by OpenAPI Typescript Codegen
```

### In your app

```tsx
// App.tsx
import {
  usePetServiceFindPetsByStatus,
} from "../openapi/queries";
function App() {
  const { data } = usePetServiceFindPetsByStatus({ status: ["available"] });

  return (
    <div className="App">
      <h1>Pet List</h1>
      <ul>
        {data?.map((pet) => (
          <li key={pet.id}>{pet.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

You can also use pure TS clients.

```tsx
import { useQuery } from "@tanstack/react-query";
import { PetService } from '../openapi/requests/services/PetService';
import {
  usePetServiceFindPetsByStatusKey,
} from "../openapi/queries";

function App() {
  // You can still use the auto-generated query key
  const { data } = useQuery([usePetServiceFindPetsByStatusKey], () => {
    // Do something here
    
    return PetService.findPetsByStatus(['available']);
  });

  return (
    <div className="App">
      {/* .... */}
    </div>
  );
}

export default App;
```

## License
MIT
