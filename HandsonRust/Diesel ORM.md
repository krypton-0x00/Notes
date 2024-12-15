To implement the schema you've provided using **Diesel ORM** in Rust, I will guide you through setting up a project from scratch. We’ll cover:

1. **Setting up the Rust project with Diesel**
2. **Creating the schema using Diesel migrations**
3. **Defining the models and relationships**
4. **Performing CRUD operations**

---

### 1. Setting up the Rust project with Diesel

Start by creating a new Rust project:

```bash
cargo new tournament_service
cd tournament_service
```

Add dependencies in `Cargo.toml`:

```toml
[dependencies]
diesel = { version = "2.0", features = ["postgres", "chrono", "serde_json"] }
dotenv = "0.15"   # For managing environment variables
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
tokio = { version = "1", features = ["full"] }

[dependencies]
# For running the Diesel CLI tool
diesel_cli = { version = "2.0", features = ["postgres"] }
```

### Install Diesel CLI

Diesel CLI helps in generating migration scripts for creating your tables. To install it, run the following:

```bash
cargo install diesel_cli --no-default-features --features postgres
```

---

### 2. Creating the schema using Diesel migrations

First, set up your PostgreSQL database. If you haven’t already, create a database:

```bash
createdb tournament_service
```

Next, you need to set up Diesel's migration system:

```bash
diesel setup
```

This command will create a `migrations` folder in your project.

Now, create a migration to define your tables:

```bash
diesel migration generate create_tournaments
```

This will generate a new folder inside the `migrations` directory. Inside this folder, edit the generated `up.sql` and `down.sql` files.

**`up.sql`** (for creating the tables):

```sql
-- Enum definitions
CREATE TYPE map_type AS ENUM ('ERANGEL', 'MIRAMAR', 'SANHOK', 'VIKENDI');
CREATE TYPE tournament_status AS ENUM ('DRAFT', 'REGISTRATION_OPEN', 'REGISTRATION_CLOSED', 'ONGOING', 'COMPLETED', 'CANCELLED');
CREATE TYPE team_registration_status AS ENUM ('PENDING', 'APPROVED', 'REJECTED');
CREATE TYPE team_player_role AS ENUM ('LEADER', 'MEMBER');

-- Table definitions
CREATE TABLE tournaments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    description TEXT,
    organization_id UUID NOT NULL,
    game_mode TEXT NOT NULL,
    map_type map_type NOT NULL,
    start_date TIMESTAMPTZ NOT NULL,
    registration_open_date TIMESTAMPTZ NOT NULL,
    registration_close_date TIMESTAMPTZ NOT NULL,
    max_teams INT NOT NULL,
    current_team_count INT DEFAULT 0,
    status tournament_status NOT NULL DEFAULT 'DRAFT'
);

CREATE TABLE prizes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tournament_id UUID NOT NULL REFERENCES tournaments(id) ON DELETE CASCADE,
    position INT NOT NULL,
    description TEXT NOT NULL,
    prize_amount FLOAT NOT NULL
);

CREATE TABLE team_registrations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tournament_id UUID NOT NULL REFERENCES tournaments(id) ON DELETE CASCADE,
    team_name TEXT NOT NULL,
    team_leader_id UUID NOT NULL,
    registration_status team_registration_status NOT NULL DEFAULT 'PENDING',
    registered_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE team_players (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    team_registration_id UUID NOT NULL REFERENCES team_registrations(id) ON DELETE CASCADE,
    player_id UUID NOT NULL,
    role team_player_role NOT NULL DEFAULT 'MEMBER'
);
```

**`down.sql`** (for dropping the tables):

```sql
DROP TABLE IF EXISTS team_players;
DROP TABLE IF EXISTS team_registrations;
DROP TABLE IF EXISTS prizes;
DROP TABLE IF EXISTS tournaments;

DROP TYPE IF EXISTS map_type;
DROP TYPE IF EXISTS tournament_status;
DROP TYPE IF EXISTS team_registration_status;
DROP TYPE IF EXISTS team_player_role;
```

Now, apply the migration:

```bash
diesel migration run
```

---

### 3. Defining the models and relationships

After running the migration, Diesel will generate Rust models for the tables. We’ll define these models manually based on the database schema.

**Create `src/schema.rs`** to define the table names and columns:

```rust
use diesel::prelude::*;

table! {
    tournaments (id) {
        id -> Nullable<Uuid>,
        name -> Varchar,
        description -> Nullable<Text>,
        organization_id -> Uuid,
        game_mode -> Varchar,
        map_type -> Varchar,
        start_date -> Timestamptz,
        registration_open_date -> Timestamptz,
        registration_close_date -> Timestamptz,
        max_teams -> Integer,
        current_team_count -> Integer,
        status -> Varchar,
    }
}

table! {
    prizes (id) {
        id -> Nullable<Uuid>,
        tournament_id -> Uuid,
        position -> Integer,
        description -> Text,
        prize_amount -> Float,
    }
}

table! {
    team_registrations (id) {
        id -> Nullable<Uuid>,
        tournament_id -> Uuid,
        team_name -> Varchar,
        team_leader_id -> Uuid,
        registration_status -> Varchar,
        registered_at -> Timestamptz,
    }
}

table! {
    team_players (id) {
        id -> Nullable<Uuid>,
        team_registration_id -> Uuid,
        player_id -> Uuid,
        role -> Varchar,
    }
}

joinable!(prizes -> tournaments (tournament_id));
joinable!(team_registrations -> tournaments (tournament_id));
joinable!(team_players -> team_registrations (team_registration_id));

allow_tables_to_appear_in_same_query!(tournaments, prizes, team_registrations, team_players);
```

**Create `src/models.rs`** to define Rust structs representing each table:

```rust
use diesel::{Queryable, Insertable};
use diesel::prelude::*;
use serde::{Serialize, Deserialize};

#[derive(Queryable, Insertable, Serialize, Deserialize, Debug)]
#[table_name = "tournaments"]
pub struct Tournament {
    pub id: Option<uuid::Uuid>,
    pub name: String,
    pub description: Option<String>,
    pub organization_id: uuid::Uuid,
    pub game_mode: String,
    pub map_type: String,
    pub start_date: chrono::NaiveDateTime,
    pub registration_open_date: chrono::NaiveDateTime,
    pub registration_close_date: chrono::NaiveDateTime,
    pub max_teams: i32,
    pub current_team_count: i32,
    pub status: String,
}

#[derive(Queryable, Insertable, Serialize, Deserialize, Debug)]
#[table_name = "prizes"]
pub struct Prize {
    pub id: Option<uuid::Uuid>,
    pub tournament_id: uuid::Uuid,
    pub position: i32,
    pub description: String,
    pub prize_amount: f64,
}

#[derive(Queryable, Insertable, Serialize, Deserialize, Debug)]
#[table_name = "team_registrations"]
pub struct TeamRegistration {
    pub id: Option<uuid::Uuid>,
    pub tournament_id: uuid::Uuid,
    pub team_name: String,
    pub team_leader_id: uuid::Uuid,
    pub registration_status: String,
    pub registered_at: chrono::NaiveDateTime,
}

#[derive(Queryable, Insertable, Serialize, Deserialize, Debug)]
#[table_name = "team_players"]
pub struct TeamPlayer {
    pub id: Option<uuid::Uuid>,
    pub team_registration_id: uuid::Uuid,
    pub player_id: uuid::Uuid,
    pub role: String,
}
```

---

### 4. Performing CRUD Operations

Now, let’s write some CRUD functions in `src/main.rs` to interact with the database.

**Connect to the database**:

```rust
use diesel::prelude::*;
use dotenv::dotenv;
use std::env;
use diesel::pg::PgConnection;

fn establish_connection() -> PgConnection {
    dotenv().ok();
    let database_url = env::var("DATABASE_URL").expect("DATABASE_URL must be set");
    PgConnection::establish(&database_url).expect(&format!("Error connecting to {}", database_url))
}
```

**Create a new tournament**:

```rust
use diesel::insert_into;

fn create_tournament(conn: &PgConnection, tournament: &Tournament) -> QueryResult<Tournament> {
    insert_into(tournaments::table)
        .values(tournament)
        .get_result(conn)
}
```

**Fetch all tournaments**:

```rust
fn get_all_tournaments(conn: &PgConnection) -> QueryResult<Vec<Tournament>> {
    tournaments::table.load::<Tournament>(conn)
}
```

Now you can test this with `cargo run` or implement more CRUD operations.

---


# Removing Columns
If you want to remove the `game_mode` and `map_type` columns from the `tournaments` table, you can do so by executing an `ALTER TABLE` SQL statement to drop those columns. Here's how you can do it:

### SQL Command to Drop Columns

```sql
ALTER TABLE tournaments
DROP COLUMN game_mode,
DROP COLUMN map_type;
```

### Explanation:

- `ALTER TABLE tournaments`: Specifies the table you want to modify (in this case, `tournaments`).
- `DROP COLUMN game_mode`: Removes the `game_mode` column from the `tournaments` table.
- `DROP COLUMN map_type`: Removes the `map_type` column from the `tournaments` table.

### Running the Command

- If you're using `psql` or any SQL client, you can directly run the above query after connecting to the PostgreSQL database.
- If you are running your migrations using Diesel or another migration tool, you will need to create a new migration that includes this SQL statement.

### Steps for Diesel (if you're using Diesel)

1. **Create a New Migration:** Run the following command to create a new migration file:
    
    ```bash
    diesel migration generate remove_game_mode_and_map_type
    ```
    
2. **Edit the Migration File:** Open the newly created migration file and modify the `up.sql` file to include the following SQL command:
    
    ```sql
    ALTER TABLE tournaments
    DROP COLUMN game_mode,
    DROP COLUMN map_type;
    ```
    
3. **Apply the Migration:** After saving the file, run the migration:
    
    ```bash
    diesel migration run
    ```
    

This will apply the changes to your database, and the `game_mode` and `map_type` columns will be removed from the `tournaments` table.

### Important Notes:

- **Data Loss**: Dropping columns will remove all the data stored in those columns, so make sure you no longer need the data or have backed it up if necessary.
- **References**: If there are any foreign keys or constraints related to those columns (e.g., if `map_type` is used in another table or there are other dependencies), you may need to adjust those first before dropping the columns.

Let me know if you need further assistance!