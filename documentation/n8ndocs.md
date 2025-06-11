TITLE: Defining a Custom n8n Vector Store Node using createVectorStoreNode (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `createVectorStoreNode` factory function to define a new n8n vector store node. It illustrates configuring metadata such as display name, description, and supported operation modes, as well as implementing the essential `getVectorStoreClient` and `populateVectorStore` asynchronous functions for client instantiation and document insertion, respectively. It also shows the optional `releaseVectorStoreClient` for resource cleanup.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/@n8n/nodes-langchain/nodes/vector_store/shared/createVectorStoreNode/README.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { createVectorStoreNode } from './createVectorStoreNode';

export class MyVectorStoreNode {
  static description = createVectorStoreNode({
    meta: {
      displayName: 'My Vector Store',
      name: 'myVectorStore',
      description: 'Operations for My Vector Store',
      docsUrl: 'https://docs.example.com/my-vector-store',
      icon: 'file:myIcon.svg',
      // Optional: specify which operations this vector store supports
      operationModes: ['load', 'insert', 'update','retrieve', 'retrieve-as-tool']
    },
    sharedFields: [
      // Fields shown in all operation modes
    ],
    loadFields: [
      // Fields specific to 'load' operation
    ],
    insertFields: [
      // Fields specific to 'insert' operation
    ],
    retrieveFields: [
      // Fields specific to 'retrieve' operation
    ],
    // Functions to implement
    getVectorStoreClient: async (context, filter, embeddings, itemIndex) => {
      // Create and return vector store instance
    },
    populateVectorStore: async (context, embeddings, documents, itemIndex) => {
      // Insert documents into vector store
    },
    // Optional: cleanup function - called in finally blocks after operations
    releaseVectorStoreClient: (vectorStore) => {
      // Release resources such as database connections or external clients
      // For example, in PGVector: vectorStore.client?.release();
    }
  });
}
```

----------------------------------------

TITLE: Customizing n8n Chat Window Styles with CSS Variables
DESCRIPTION: This CSS snippet defines a comprehensive set of CSS variables (`--chat--*`) that allow for extensive customization of the n8n Chat window's appearance. These variables control colors, spacing, border-radius, transition durations, window dimensions, header styles, textarea height, message styling (font size, padding, colors for bot/user messages), and toggle button styles. Developers can override these variables to match their application's design system.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/chat/README.md#_snippet_6

LANGUAGE: CSS
CODE:
```
:root {
	--chat--color-primary: #e74266;
	--chat--color-primary-shade-50: #db4061;
	--chat--color-primary-shade-100: #cf3c5c;
	--chat--color-secondary: #20b69e;
	--chat--color-secondary-shade-50: #1ca08a;
	--chat--color-white: #ffffff;
	--chat--color-light: #f2f4f8;
	--chat--color-light-shade-50: #e6e9f1;
	--chat--color-light-shade-100: #c2c5cc;
	--chat--color-medium: #d2d4d9;
	--chat--color-dark: #101330;
	--chat--color-disabled: #777980;
	--chat--color-typing: #404040;

	--chat--spacing: 1rem;
	--chat--border-radius: 0.25rem;
	--chat--transition-duration: 0.15s;

	--chat--window--width: 400px;
	--chat--window--height: 600px;

	--chat--header-height: auto;
	--chat--header--padding: var(--chat--spacing);
	--chat--header--background: var(--chat--color-dark);
	--chat--header--color: var(--chat--color-light);
	--chat--header--border-top: none;
	--chat--header--border-bottom: none;
	--chat--header--border-bottom: none;
	--chat--header--border-bottom: none;
	--chat--heading--font-size: 2em;
	--chat--header--color: var(--chat--color-light);
	--chat--subtitle--font-size: inherit;
	--chat--subtitle--line-height: 1.8;

	--chat--textarea--height: 50px;

	--chat--message--font-size: 1rem;
	--chat--message--padding: var(--chat--spacing);
	--chat--message--border-radius: var(--chat--border-radius);
	--chat--message-line-height: 1.8;
	--chat--message--bot--background: var(--chat--color-white);
	--chat--message--bot--color: var(--chat--color-dark);
	--chat--message--bot--border: none;
	--chat--message--user--background: var(--chat--color-secondary);
	--chat--message--user--color: var(--chat--color-white);
	--chat--message--user--border: none;
	--chat--message--pre--background: rgba(0, 0, 0, 0.05);

	--chat--toggle--background: var(--chat--color-primary);
	--chat--toggle--hover--background: var(--chat--color-primary-shade-50);
	--chat--toggle--active--background: var(--chat--color-primary-shade-100);
	--chat--toggle--color: var(--chat--color-white);
	--chat--toggle--size: 64px;
}
```

----------------------------------------

TITLE: Installing Project Dependencies with pnpm
DESCRIPTION: This command installs all required project dependencies using the pnpm package manager. It is typically the initial step after cloning the repository to prepare the development environment.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/design-system/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
pnpm install
```

----------------------------------------

TITLE: Integrating n8n Chat in React
DESCRIPTION: This React snippet illustrates how to integrate the n8n Chat widget into a React functional component. The `createChat` function is invoked within a `useEffect` hook with an empty dependency array to ensure it runs only once after the initial render.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/chat/README.md#_snippet_4

LANGUAGE: tsx
CODE:
```
// App.tsx
import { useEffect } from 'react';
import '@n8n/chat/style.css';
import { createChat } from '@n8n/chat';

export const App = () => {
	useEffect(() => {
		createChat({
			webhookUrl: 'YOUR_PRODUCTION_WEBHOOK_URL'
		});
	}, []);

	return (<div></div>);
};
```

----------------------------------------

TITLE: Quick Starting n8n with npx (Node.js)
DESCRIPTION: This command allows users to instantly try n8n using npx, a Node.js package runner. It requires Node.js to be installed on the system. This is a quick way to launch a temporary n8n instance for testing or initial setup.
SOURCE: https://github.com/n8n-io/n8n/blob/master/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
npx n8n
```

----------------------------------------

TITLE: Starting n8n in Docker
DESCRIPTION: This command initializes a Docker volume for persistent data storage and then runs the n8n Docker container. It maps port 5678, mounts the n8n_data volume to store workflow data and credentials, and ensures n8n is accessible locally at http://localhost:5678.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
docker volume create n8n_data

docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n
```

----------------------------------------

TITLE: Deploying n8n with Docker
DESCRIPTION: These commands demonstrate how to deploy n8n using Docker. The first command creates a named Docker volume to persist n8n data, ensuring data is not lost when the container is removed. The second command runs n8n in a Docker container, mapping port 5678 and mounting the created volume for data persistence. It's a common method for self-hosting n8n.
SOURCE: https://github.com/n8n-io/n8n/blob/master/README.md#_snippet_1

LANGUAGE: Shell
CODE:
```
docker volume create n8n_data
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

----------------------------------------

TITLE: Initializing n8n Chat with TypeScript Import
DESCRIPTION: This TypeScript snippet shows how to import the n8n Chat stylesheet and the `createChat` function, then initialize the chat widget with your n8n webhook URL. This method is suitable for projects using module bundlers.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/chat/README.md#_snippet_2

LANGUAGE: ts
CODE:
```
import '@n8n/chat/style.css';
import { createChat } from '@n8n/chat';

createChat({
	webhookUrl: 'YOUR_PRODUCTION_WEBHOOK_URL'
});
```

----------------------------------------

TITLE: Updating n8n with Docker Compose
DESCRIPTION: These commands collectively update an n8n instance managed by Docker Compose. First, `docker compose pull` fetches the latest images. Then, `docker compose down` stops and removes the old containers. Finally, `docker compose up -d` starts new containers with the updated images in detached mode.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_10

LANGUAGE: bash
CODE:
```
# Pull latest version
docker compose pull
```

LANGUAGE: bash
CODE:
```
# Stop and remove older version
docker compose down
```

LANGUAGE: bash
CODE:
```
# Start the container
docker compose up -d
```

----------------------------------------

TITLE: Migrating Binary Data Access in n8n (TypeScript)
DESCRIPTION: This example illustrates the updated approach for accessing binary data in n8n from version 0.135.0. Direct access to `binaryData.data` is deprecated; instead, the `await this.helpers.getBinaryDataBuffer(itemIndex, binaryPropertyName)` method must be used to retrieve binary data as a Buffer. This change standardizes binary data handling within workflows.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md#_snippet_2

LANGUAGE: typescript
CODE:
```
const items = this.getInputData();

for (const i = 0; i < items.length; i++) {
	const item = items[i].binary as IBinaryKeyData;
	const binaryPropertyName = this.getNodeParameter('binaryPropertyName', i);
	const binaryData = item[binaryPropertyName] as IBinaryData;
	// Before 0.135.0:
	const binaryDataBuffer = Buffer.from(binaryData.data, BINARY_ENCODING);
	// From 0.135.0:
	const binaryDataBuffer = await this.helpers.getBinaryDataBuffer(i, binaryPropertyName);
}
```

----------------------------------------

TITLE: Initializing Vue.js Application with Theme and Component Data
DESCRIPTION: This Vue.js application setup initializes the Vue devtools and defines the root component's data and watchers. The 'data' property holds various UI component states (e.g., radio buttons, inputs, tree data, select options, table data) and theme-related variables. The 'watch' property dynamically updates CSS variables based on changes to a 'global' object or individual color properties, demonstrating a reactive theme system.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/design-system/theme/preview/index.html#_snippet_10

LANGUAGE: Vue.js
CODE:
```
Vue.config.devtools = true; Vue.createApp({
  data() {
    return {
      global: {},
      colorLine: ['Primary', 'Success', 'Warning', 'Danger', 'Info'],
      color_primary: '',
      color_success: '',
      color_warning: '',
      color_danger: '',
      color_info: '',
      color_white: '',
      color_black: '',
      color_text_primary: '',
      color_text_regular: '',
      color_text_secondary: '',
      color_text_placeholder: '',
      border_color_base: '',
      border_color_light: '',
      border_color_lighter: '',
      border_color_extra_light: '',
      font_size_extra_large: '',
      font_size_large: '',
      font_size_medium: '',
      font_size_base: '',
      font_size_small: '',
      font_size_extra_small: '',
      font_weight_primary: 0,
      font_weight_secondary: 0,
      font_line_height_primary: '',
      font_line_height_secondary: '',
      radio: '1',
      radio1: 'Washington',
      radio2: '1',
      checked: true,
      checked1: ['Shanghai'],
      checked2: true,
      input: 'Element',
      inputNumber: 1,
      treeData: [
        {
          label: '一级 1',
          children: [
            {
              label: '二级 1-1',
              children: [
                {
                  label: '三级 1-1-1',
                }
              ]
            }
          ]
        },
        {
          label: '一级 2',
          children: [
            {
              label: '二级 2-1',
              children: [
                {
                  label: '三级 2-1-1',
                }
              ]
            },
            {
              label: '二级 2-2',
              children: [
                {
                  label: '三级 2-2-1',
                }
              ]
            }
          ]
        },
        {
          label: '一级 3',
          children: [
            {
              label: '二级 3-1',
              children: [
                {
                  label: '三级 3-1-1',
                }
              ]
            },
            {
              label: '二级 3-2',
              children: [
                {
                  label: '三级 3-2-1',
                }
              ]
            }
          ]
        }
      ],
      selectOptions: [
        {
          value: 'Option1',
          label: 'Option1'
        },
        {
          value: 'Option2',
          label: 'Option2'
        },
        {
          value: 'Option3',
          label: 'Option3'
        },
        {
          value: 'Option4',
          label: 'Option4'
        },
        {
          value: 'Option5',
          label: 'Option5'
        }
      ],
      selectValue: '',
      cascadeOptions: [
        {
          value: 'guide',
          label: 'Guide',
          children: [
            {
              value: 'disciplines',
              label: 'Disciplines',
              children: [
                {
                  value: 'consistency',
                  label: 'Consistency'
                },
                {
                  value: 'feedback',
                  label: 'Feedback'
                }
              ]
            }
          ]
        },
        {
          value: 'resource',
          label: 'Resource',
          children: [
            {
              value: 'axure',
              label: 'Axure Components'
            },
            {
              value: 'sketch',
              label: 'Sketch Templates'
            },
            {
              value: 'docs',
              label: 'Design Documentation'
            }
          ]
        }
      ],
      cascaderValue: [],
      switchValue: true,
      slider: 28,
      datePicker: '',
      rate: null,
      transferData: (() => {
        const data = [];
        for (let i = 1; i <= 15; i++) {
          data.push({
            key: i,
            label: `Option ${i}`,
            disabled: i % 4 === 0
          });

```

----------------------------------------

TITLE: Stopping a Docker Container by ID
DESCRIPTION: This command stops a running Docker container using its unique `container_id`. It gracefully shuts down the container, allowing it to save its state if configured to do so.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_7

LANGUAGE: bash
CODE:
```
docker stop [container_id]
```

----------------------------------------

TITLE: Creating a Basic n8n Regular Node in TypeScript
DESCRIPTION: This TypeScript code defines a basic n8n regular node named 'My Node'. It implements the `INodeType` interface, setting up display information, inputs, outputs, and a custom string property. The `execute` method iterates through input items, adds the value of 'myString' to each item's JSON data, and returns the modified items. This example demonstrates how to define node properties and process data within a node.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/node-dev/README.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import {
	IExecuteFunctions,
	INodeExecutionData,
	INodeType,
	INodeTypeDescription,
} from 'n8n-workflow';


export class MyNode implements INodeType {
	description: INodeTypeDescription = {
		displayName: 'My Node',
		name: 'myNode',
		group: ['transform'],
		version: 1,
		description: 'Adds "myString" on all items to defined value.',
		defaults: {
			name: 'My Node',
			color: '#772244',
		},
		inputs: ['main'],
		outputs: ['main'],
		properties: [
			// Node properties which the user gets displayed and
			// can change on the node.
			{
				displayName: 'My String',
				name: 'myString',
				type: 'string',
				default: '',
				placeholder: 'Placeholder value',
				description: 'The description text',
			}
		]
	};


	async execute(this: IExecuteFunctions): Promise<INodeExecutionData[][]> {

		const items = this.getInputData();

		let item: INodeExecutionData;
		let myString: string;

		// Itterates over all input items and add the key "myString" with the
		// value the parameter "myString" resolves to.
		// (This could be a different value for each item in case it contains an expression)
		for (let itemIndex = 0; itemIndex < items.length; itemIndex++) {
			myString = this.getNodeParameter('myString', itemIndex, '') as string;
			item = items[itemIndex];

			item.json['myString'] = myString;
		}

		return [items];

	}
}
```

----------------------------------------

TITLE: Exporting n8n Workflows and Credentials (CLI)
DESCRIPTION: These commands are used to export all n8n workflows and credentials to a specified backup directory using the n8n CLI. This is a crucial step before upgrading n8n, especially when migrating from MongoDB to a different database, to ensure data preservation.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md#_snippet_3

LANGUAGE: bash
CODE:
```
n8n export:workflow --backup --output=backups/latest/
n8n export:credentials --backup --output=backups/latest/
```

----------------------------------------

TITLE: Setting Up Project Dependencies with pnpm
DESCRIPTION: This command installs all project dependencies listed in `package.json` using pnpm, which is a fast and efficient package manager.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/editor-ui/README.md#_snippet_1

LANGUAGE: Shell
CODE:
```
pnpm install
```

----------------------------------------

TITLE: Implementing Resource Cleanup with `finally` Block in TypeScript
DESCRIPTION: This TypeScript `try...finally` block illustrates the critical pattern for ensuring proper resource cleanup in vector store operations. The `releaseVectorStoreClient` function is invoked within the `finally` block, guaranteeing that resources, such as database connections, are released regardless of whether the operation logic succeeds or encounters an error. This prevents resource leaks and connection pool exhaustion, especially important for database-backed vector stores.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/@n8n/nodes-langchain/nodes/vector_store/shared/createVectorStoreNode/README.md#_snippet_4

LANGUAGE: TypeScript
CODE:
```
try {
  // Operation logic
} finally {
  // Release resources even if an error occurs
  args.releaseVectorStoreClient?.(vectorStore);
}
```

----------------------------------------

TITLE: Creating and Populating Employee Table in SQL
DESCRIPTION: This snippet defines the `emp` table schema, including columns for employee number, name, job, manager, hire date, salary, commission, and department. It then inserts 14 sample records into the `emp` table, providing initial data for an employee database.
SOURCE: https://github.com/n8n-io/n8n/blob/master/cypress/fixtures/Dummy_sql.txt#_snippet_0

LANGUAGE: SQL
CODE:
```
CREATE TABLE emp (
empno INT PRIMARY KEY,
ename VARCHAR(10),
job VARCHAR(9),
mgr INT NULL,
hiredate DATETIME,
sal NUMERIC(7,2),
comm NUMERIC(7,2) NULL,
dept INT)
begin
insert into emp values
    (1,'JOHNSON','ADMIN',6,'12-17-1990',18000,NULL,4)
insert into emp values
    (2,'HARDING','MANAGER',9,'02-02-1998',52000,300,3)
insert into emp values
    (3,'TAFT','SALES I',2,'01-02-1996',25000,500,3)
insert into emp values
    (4,'HOOVER','SALES I',2,'04-02-1990',27000,NULL,3)
insert into emp values
    (5,'LINCOLN','TECH',6,'06-23-1994',22500,1400,4)
insert into emp values
    (6,'GARFIELD','MANAGER',9,'05-01-1993',54000,NULL,4)
insert into emp values
    (7,'POLK','TECH',6,'09-22-1997',25000,NULL,4)
insert into emp values
    (8,'GRANT','ENGINEER',10,'03-30-1997',32000,NULL,2)
insert into emp values
    (9,'JACKSON','CEO',NULL,'01-01-1990',75000,NULL,4)
insert into emp values
    (10,'FILLMORE','MANAGER',9,'08-09-1994',56000,NULL,2)
insert into emp values
    (11,'ADAMS','ENGINEER',10,'03-15-1996',34000,NULL,2)
insert into emp values
    (12,'WASHINGTON','ADMIN',6,'04-16-1998',18000,NULL,4)
insert into emp values
    (13,'MONROE','ENGINEER',10,'12-03-2000',30000,NULL,2)
insert into emp values
    (14,'ROOSEVELT','CPA',9,'10-12-1995',35000,NULL,1)
end
```

----------------------------------------

TITLE: Converting JSON Schema to Zod Schema in TypeScript
DESCRIPTION: This example shows how to use the `jsonSchemaToZod` function to convert a basic JSON schema object into a Zod schema. It imports the function and defines a simple JSON schema for an object with a 'hello' string property, then converts it.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/@n8n/json-schema-to-zod/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { jsonSchemaToZod } from "json-schema-to-zod";

const jsonSchema = {
  type: "object",
  properties: {
    hello: {
      type: "string",
    },
  },
};

const zodSchema = jsonSchemaToZod(myObject);
```

----------------------------------------

TITLE: Installing n8n Globally via npm
DESCRIPTION: This command installs the n8n workflow automation platform globally on your system using npm, making the `n8n` command available from any directory.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/editor-ui/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install n8n -g
```

----------------------------------------

TITLE: Starting n8n via New Binary Path (Recommended) - Shell
DESCRIPTION: This command demonstrates the updated and recommended method for starting n8n after the CLI library upgrade. Users should modify their existing n8n startup commands or scripts to point to this new binary path to ensure proper application launch and security compliance.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md#_snippet_9

LANGUAGE: Shell
CODE:
```
/usr/local/bin/node bin/n8n start
```

----------------------------------------

TITLE: Running n8n with Specific Timezone Settings
DESCRIPTION: This command runs an n8n Docker container and sets its timezone using the `GENERIC_TIMEZONE` and `TZ` environment variables. `GENERIC_TIMEZONE` affects n8n's internal scheduling (e.g., Schedule node), while `TZ` sets the system's timezone for commands and scripts within the container.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_11

LANGUAGE: bash
CODE:
```
docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -e GENERIC_TIMEZONE="Europe/Berlin" \
 -e TZ="Europe/Berlin" \
 docker.n8n.io/n8nio/n8n
```

----------------------------------------

TITLE: Defining Labels Collection Parameter in n8n Node (JavaScript)
DESCRIPTION: This JavaScript object defines a 'collection' type parameter named 'labels'. It allows multiple values, indicated by `multipleValues: true`, and specifies a button text for adding new labels. The `displayOptions` ensure it only shows for 'create' operation on 'issue' resource. It contains a nested 'label' string parameter.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/i18n/docs/README.md#_snippet_18

LANGUAGE: JavaScript
CODE:
```
{
	displayName: 'Labels',
	name: 'labels', // key to use in translation
	type: 'collection',
	typeOptions: {
		multipleValues: true,
		multipleValueButtonText: 'Add Label',
	},
	displayOptions: {
		show: {
			operation: [
				'create',
			],
			resource: [
				'issue',
			],
		},
	},
	default: { 'label': '' },
	options: [
		{
			displayName: 'Label',
			name: 'label', // key to use in translation
			type: 'string',
			default: '',
			description: 'Label to add to issue',
		},
	],
},
```

----------------------------------------

TITLE: Cloning n8n Repository - Git
DESCRIPTION: Clones your forked n8n repository from GitHub to your local machine. Replace `<your_github_username>` with your actual GitHub username to get started with the development setup.
SOURCE: https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md#_snippet_8

LANGUAGE: Shell
CODE:
```
git clone https://github.com/<your_github_username>/n8n.git
```

----------------------------------------

TITLE: Implementing Dependency Injection with @n8n/di in TypeScript
DESCRIPTION: This TypeScript example demonstrates how to use the `@Service()` decorator to register classes for dependency injection and how to retrieve instances using `Container.get()`. It shows automatic injection of `ExampleInjectedService` into `ExampleService`'s constructor, enabling modular and testable code. This pattern is derived from `typedi`'s usage.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/@n8n/di/README.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
// from https://github.com/typestack/typedi/blob/develop/README.md
import { Container, Service } from 'typedi';

@Service()
class ExampleInjectedService {
  printMessage() {
    console.log('I am alive!');
  }
}

@Service()
class ExampleService {
  constructor(
    // because we annotated ExampleInjectedService with the @Service()
    // decorator TypeDI will automatically inject an instance of
    // ExampleInjectedService here when the ExampleService class is requested
    // from TypeDI.
    public injectedService: ExampleInjectedService
  ) {}
}

const serviceInstance = Container.get(ExampleService);
// we request an instance of ExampleService from TypeDI

serviceInstance.injectedService.printMessage();
// logs "I am alive!" to the console
```

----------------------------------------

TITLE: Starting n8n in Development Mode - pnpm
DESCRIPTION: Initiates n8n in development mode, which automatically rebuilds code, restarts the backend, and refreshes the frontend (editor-ui) on every change. This provides an efficient workflow for iterating on n8n modules.
SOURCE: https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md#_snippet_15

LANGUAGE: Shell
CODE:
```
pnpm dev
```

----------------------------------------

TITLE: Pulling Latest Stable n8n Docker Image
DESCRIPTION: This command pulls the most recent stable version of the n8n Docker image from the official n8n Docker registry. It is used to update your local n8n image to the latest release.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_3

LANGUAGE: bash
CODE:
```
docker pull docker.n8n.io/n8nio/n8n
```

----------------------------------------

TITLE: Installing n8n-node-dev CLI
DESCRIPTION: This command installs the n8n-node-dev command-line interface globally using npm, making it available for use from any directory on the system. It is the first step to set up the development environment for n8n custom nodes and credentials.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/node-dev/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install n8n-node-dev -g
```

----------------------------------------

TITLE: Building All n8n Project Code - pnpm
DESCRIPTION: Compiles and builds all the code across the n8n project. This step is necessary to ensure all components are ready for execution or further development, especially after installing dependencies.
SOURCE: https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md#_snippet_12

LANGUAGE: Shell
CODE:
```
pnpm build
```

----------------------------------------

TITLE: Renaming `evaluateExpression` to `$evaluateExpression` in n8n Functions
DESCRIPTION: This snippet illustrates the required change for n8n Function and FunctionItem Nodes where the internal utility function `evaluateExpression(...)` has been renamed to `$evaluateExpression()` for better code normalization and simplification.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
// Old usage:
evaluateExpression(...)

// New usage:
$evaluateExpression(...)
```

----------------------------------------

TITLE: Installing n8n Project Dependencies - pnpm
DESCRIPTION: Installs all required dependencies for all modules within the n8n project using pnpm. This command also links the modules together, preparing the environment for development.
SOURCE: https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md#_snippet_11

LANGUAGE: Shell
CODE:
```
pnpm install
```

----------------------------------------

TITLE: Applying Dark Mode Styles with CSS
DESCRIPTION: This CSS snippet applies a dark background color to the body element when the user's system prefers a dark color scheme. It ensures a consistent visual experience in dark mode.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/editor-ui/index.html#_snippet_0

LANGUAGE: CSS
CODE:
```
@media (prefers-color-scheme: dark) { body { background-color: rgb(45, 46, 46) } }
```

----------------------------------------

TITLE: Adjusting Slack Node Data Access Expression (n8n Expression)
DESCRIPTION: This snippet demonstrates the required change in n8n expressions for accessing data from the Slack Node's 'Channel -> Create' operation. Previously, the channel ID was nested under a 'channel' property; now, it is directly available at the main level of the node's output data, requiring updates to existing references.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md#_snippet_5

LANGUAGE: n8n Expression
CODE:
```
{{ $node["Slack"].data["channel"]["id"] }}
```

LANGUAGE: n8n Expression
CODE:
```
{{ $node["Slack"].data["id"] }}
```

----------------------------------------

TITLE: Running n8n with PostgreSQL using Docker
DESCRIPTION: This command initializes a Docker volume for n8n data and then runs an n8n container configured to use PostgreSQL as its database. It requires placeholders for PostgreSQL connection details (database, host, port, user, schema, password) to be replaced with actual values. The `n8n_data` volume ensures persistence of user data and encryption keys.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_2

LANGUAGE: bash
CODE:
```
docker volume create n8n_data

docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -e DB_TYPE=postgresdb \
 -e DB_POSTGRESDB_DATABASE=<POSTGRES_DATABASE> \
 -e DB_POSTGRESDB_HOST=<POSTGRES_HOST> \
 -e DB_POSTGRESDB_PORT=<POSTGRES_PORT> \
 -e DB_POSTGRESDB_USER=<POSTGRES_USER> \
 -e DB_POSTGRESDB_SCHEMA=<POSTGRES_SCHEMA> \
 -e DB_POSTGRESDB_PASSWORD=<POSTGRES_PASSWORD> \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n
```

----------------------------------------

TITLE: Starting a New n8n Docker Container
DESCRIPTION: This command starts a new n8n Docker container. It allows specifying a custom `container_name` and additional `options` (e.g., port mappings, environment variables) to configure the container. The `-d` flag runs the container in detached mode.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_9

LANGUAGE: bash
CODE:
```
docker run --name=[container_name] [options] -d docker.n8n.io/n8nio/n8n
```

----------------------------------------

TITLE: Updating Asynchronous Binary Stream Handling in n8n Nodes (TypeScript)
DESCRIPTION: This snippet demonstrates the change in `this.helpers.getBinaryStream()` from synchronous to asynchronous in n8n nodes. Users must now prepend `await` when calling this helper to ensure proper handling of the returned Promise, preventing potential issues with binary data streams. This change affects custom nodes interacting with binary data.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
const binaryStream = this.helpers.getBinaryStream(id); // until 1.9.0
const binaryStream = await this.helpers.getBinaryStream(id); // since 1.9.0
```

----------------------------------------

TITLE: Starting n8n with Tunnel for Local Development
DESCRIPTION: This command starts n8n in a Docker container with the --tunnel flag, enabling it to be reachable from the internet via a special tunnel service. This setup is intended for local development and testing purposes, allowing webhooks to function correctly without requiring a public IP address.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
docker volume create n8n_data

docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n \
 start --tunnel
```

----------------------------------------

TITLE: Integrating n8n Chat in Vue.js
DESCRIPTION: This Vue.js snippet demonstrates how to integrate the n8n Chat widget within a Vue component using the Composition API. The `createChat` function is called within `onMounted` to ensure the DOM is ready before initialization.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/chat/README.md#_snippet_3

LANGUAGE: html
CODE:
```
<script lang="ts" setup>
// App.vue
import { onMounted } from 'vue';
import '@n8n/chat/style.css';
import { createChat } from '@n8n/chat';

onMounted(() => {
	createChat({
		webhookUrl: 'YOUR_PRODUCTION_WEBHOOK_URL'
	});
});
</script>
<template>
	<div></div>
</template>
```

----------------------------------------

TITLE: Updating Asynchronous Credential Retrieval in n8n (TypeScript)
DESCRIPTION: This snippet demonstrates the change in the `getCredentials` method from synchronous to asynchronous in n8n version 0.135.0. Users must now `await` the call to `this.getCredentials` to properly handle the returned Promise, ensuring correct credential retrieval in asynchronous contexts.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md#_snippet_1

LANGUAGE: typescript
CODE:
```
// Before 0.135.0:
const credentials = this.getCredentials(credentialTypeName);

// From 0.135.0:
const credentials = await this.getCredentials(myNodeCredentials);
```

----------------------------------------

TITLE: Linting and Fixing Files with pnpm
DESCRIPTION: This command lints the project files to enforce code style and quality standards, and automatically fixes any fixable issues. It helps maintain code consistency and identify potential errors.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/design-system/README.md#_snippet_4

LANGUAGE: Shell
CODE:
```
pnpm lint
```

----------------------------------------

TITLE: Enabling Node.js Corepack
DESCRIPTION: This command enables Node.js corepack, a feature that allows managing package managers like pnpm, ensuring the correct version is used across the project.
SOURCE: https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md#_snippet_3

LANGUAGE: shell
CODE:
```
corepack enable
```

----------------------------------------

TITLE: Enabling and Activating pnpm on Windows (Admin)
DESCRIPTION: These commands enable corepack and then prepare and activate pnpm on Windows. It is crucial to run these commands in a terminal with administrator privileges for them to execute successfully.
SOURCE: https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md#_snippet_7

LANGUAGE: shell
CODE:
```
corepack enable
corepack prepare pnpm --activate
```

----------------------------------------

TITLE: Adding Upstream Remote to n8n Repository - Git
DESCRIPTION: Adds the original n8n repository as an 'upstream' remote to your forked repository. This configuration allows you to easily synchronize your fork with the latest changes from the main n8n project.
SOURCE: https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md#_snippet_10

LANGUAGE: Shell
CODE:
```
git remote add upstream https://github.com/n8n-io/n8n.git
```

----------------------------------------

TITLE: Creating a Dynamic Tool for Vector Store Functionality in TypeScript
DESCRIPTION: This TypeScript snippet demonstrates how a `DynamicTool` is instantiated for the `retrieve-as-tool` mode, exposing vector store capabilities. The tool is configured with a `name`, `description`, and an asynchronous `func` that encapsulates the logic for searching the vector store based on the provided input. This allows vector store operations to be integrated as callable tools within a larger system.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/@n8n/nodes-langchain/nodes/vector_store/shared/createVectorStoreNode/README.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
const vectorStoreTool = new DynamicTool({
  name: toolName,
  description: toolDescription,
  func: async (input) => {
    // Search vector store with input
    // ...
  },
});
```

----------------------------------------

TITLE: Embedding n8n Chat via CDN
DESCRIPTION: This snippet demonstrates how to embed the n8n Chat widget directly into an HTML page using CDN links. It includes linking the necessary CSS stylesheet and initializing the chat functionality with a specified webhook URL using a JavaScript module import.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/chat/README.md#_snippet_0

LANGUAGE: html
CODE:
```
<link href="https://cdn.jsdelivr.net/npm/@n8n/chat/dist/style.css" rel="stylesheet" />
<script type="module">
	import { createChat } from 'https://cdn.jsdelivr.net/npm/@n8n/chat/dist/chat.bundle.es.js';

	createChat({
		webhookUrl: 'YOUR_PRODUCTION_WEBHOOK_URL'
	});
</script>
```

----------------------------------------

TITLE: Building n8n Docker Image with Buildx
DESCRIPTION: This command builds a multi-platform n8n Docker image using `docker buildx`. It allows specifying the `N8N_VERSION` as a build argument and tags the resulting image with the specified version. This is used for custom builds of n8n Docker images.
SOURCE: https://github.com/n8n-io/n8n/blob/master/docker/images/n8n/README.md#_snippet_12

LANGUAGE: bash
CODE:
```
docker buildx build --platform linux/amd64,linux/arm64 --build-arg N8N_VERSION=<VERSION> -t n8n:<VERSION> .
```

LANGUAGE: bash
CODE:
```
# For example:
docker buildx build --platform linux/amd64,linux/arm64 --build-arg N8N_VERSION=1.30.1 -t n8n:1.30.1 .
```

----------------------------------------

TITLE: Defining Interpolated Strings in n8n i18n JSON
DESCRIPTION: This JSON snippet illustrates the use of interpolation in n8n i18n strings. Variables enclosed in curly braces, like `{activeExecutionId}`, are placeholders that will be dynamically replaced with values at runtime, enabling flexible message construction.
SOURCE: https://github.com/n8n-io/n8n/blob/master/packages/frontend/@n8n/i18n/docs/ADDENDUM.md#_snippet_1

LANGUAGE: json
CODE:
```
{
	"stopExecution.message": "The execution with the ID {activeExecutionId} got stopped!",
	"stopExecution.title": "Execution stopped"
}
```