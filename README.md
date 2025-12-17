<div align="center">
  <img src="assets/Energy ID.png" alt="Energy ID Logo" width="200"/>
</div>

# Schema Builder

A visual, interactive tool for building Verifiable Credential (VC) schemas in both JSON Schema and JSON-LD formats. This Next.js application provides an intuitive interface to define credential schemas with nested attributes, preview the output in real-time, and save them to a MongoDB database.

## Features

- **Visual Schema Builder**: Create credential schemas through an intuitive two-step wizard interface
- **Tree-Based Attribute Editor**: Define nested attributes with a hierarchical tree structure
- **Dual Format Support**: Generate schemas in both JSON Schema and JSON-LD formats
- **Real-Time Preview**: See your schema output as you build it
- **Database Integration**: Save and retrieve schemas from MongoDB
- **Export Options**: Copy to clipboard or download schemas as JSON files
- **Type Support**: Define attributes with types: string, number, boolean, object, and integer
- **Required Field Management**: Mark attributes as required or optional
- **Credential Type Selection**: Choose between Merklized and Non-merklized credential types

## Tech Stack

- **Framework**: [Next.js 15](https://nextjs.org/) (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: 
  - Radix UI (Accordion, Checkbox, Label, Radio Group, Select)
  - Ant Design (Tree component)
  - Custom shadcn/ui components
- **Form Management**: React Hook Form with Zod validation
- **Database**: MongoDB
- **Icons**: Lucide React, Ant Design Icons
- **Notifications**: React Hot Toast

## Getting Started

### Prerequisites

- Node.js 18+ and npm/yarn/pnpm
- MongoDB database (local or cloud instance)
- Environment variables configured (see [Environment Variables](#environment-variables))

### Installation

1. Clone the repository:
```bash
git clone https://github.com/impactility/schema-builder
cd schema-builder
```

2. Install dependencies:
```bash
npm install
# or
yarn install
# or
pnpm install
```

3. Set up environment variables:
Create a `.env.local` file in the root directory:
```env
MONGODB_URI=your_mongodb_connection_string
HOST_URL=http://localhost:5006
```

4. Run the development server:
```bash
npm run dev
# or
yarn dev
# or
pnpm dev
```

5. Open [http://localhost:5006](http://localhost:5006) in your browser.

## Project Structure

```
schema-builder/
├── app/
│   ├── (builder)/
│   │   ├── components/
│   │   │   ├── helper.ts          # JSON/JSON-LD conversion logic
│   │   │   ├── preview.tsx        # Schema preview component
│   │   │   ├── renderer.tsx       # Main renderer with step management
│   │   │   ├── step1.tsx          # Step 1: Schema metadata form
│   │   │   └── step2.tsx          # Step 2: Attribute tree editor
│   │   └── page.tsx               # Main builder page
│   ├── api/
│   │   └── schemas/
│   │       ├── route.ts           # GET/POST all schemas
│   │       ├── json/[id]/
│   │       │   └── route.ts       # GET JSON schema by ID
│   │       └── jsonLd/[id]/
│   │           └── route.ts       # GET JSON-LD schema by ID
│   ├── layout.tsx                 # Root layout
│   └── globals.css                # Global styles
├── components/
│   └── ui/                        # Reusable UI components
├── lib/
│   ├── mongodb.ts                 # MongoDB connection utility
│   └── utils.ts                   # Utility functions
├── assets/
│   └── Energy ID.png              # Logo
└── public/                        # Static assets
```

## Usage

### Step 1: Define Schema Metadata

1. Enter the **Title** of your schema
2. Specify the **Schema Type** (no spaces allowed)
3. Set the **Version** number
4. Provide a **Description**
5. (Optional) Expand "Advanced options" to select **Credential Type**:
   - Merklized
   - Non-merklized
6. Click "Define attributes" to proceed

### Step 2: Define Attributes

1. Use the tree view to navigate and select attributes
2. Click on an attribute to edit its properties:
   - **Name**: Attribute identifier (used in the schema)
   - **Title**: Human-readable title
   - **Data Type**: string, number, boolean, object, or integer
   - **Description**: Attribute description
   - **Required**: Toggle to mark as required
3. Click "Add" to add a child attribute (only for object types)
4. Click "Remove" to delete an attribute (cannot remove "id" or "credentialSubject")
5. Click "Save" to update the attribute
6. Use "Back" to return to Step 1

### Preview and Export

- **Format Toggle**: Switch between JSON and JSON-LD preview
- **Copy**: Copy the schema to clipboard
- **Download**: Download the schema as a JSON file
- **Save**: Save the schema to MongoDB

## API Endpoints

### GET `/api/schemas`
Retrieve all saved schemas.

**Response:**
```json
{
  "message": "Data fetched successfully",
  "data": [...]
}
```

### POST `/api/schemas`
Create a new schema.

**Request Body:**
```json
{
  "name": "Schema Name",
  "schemaId": "unique-schema-id",
  "json": {...},
  "jsonLd": {...},
  "orgId": "organization-id"
}
```

**Response:**
```json
{
  "message": "Schema created successfully",
  "result": {...}
}
```

### GET `/api/schemas/json/[id]`
Retrieve a JSON schema by ID.

**Response:**
```json
{
  // JSON schema object
}
```

### GET `/api/schemas/jsonLd/[id]`
Retrieve a JSON-LD schema by ID.

**Response:**
```json
{
  // JSON-LD schema object
}
```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `MONGODB_URI` | MongoDB connection string | Yes |
| `HOST_URL` | Base URL for the application (used in JSON-LD context URLs) | Yes |

## Database Schema

Schemas are stored in MongoDB in the `requests` database, `vc-schemas` collection with the following structure:

```typescript
{
  name: string;
  schemaId: string;
  json: object;        // JSON Schema format
  jsonLd: object;      // JSON-LD format
  orgId: string;
  title?: string;
  schemaType?: string;
  version?: string;
  description?: string;
  credentialType?: "Merklized" | "Non-merklized";
}
```

## Building for Production

```bash
npm run build
npm start
```

The production server will run on port 5006.

## Development

The development server runs on port 5006 by default. You can modify this in `package.json`:

```json
{
  "scripts": {
    "dev": "next dev -p 5006",
    "start": "next start -p 5006"
  }
}
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.  
Copyright (c) 2025 Impactility
