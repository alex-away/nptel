# Web Application Testing

## 1. What are Web Applications?

A **web application** is a program deployed on a web server that users interact with through a web browser. They typically use HTML for the user interface and communicate via the HTTP protocol.

- **Web Services** are similar but usually have no direct human UI; they communicate using XML messages (often via SOAP).

- **Characteristics**: Web apps are often composed of loosely coupled components, communicate via messages, are inherently concurrent and distributed, and use a mix of technologies (JSP, JavaScript, PHP, etc.).

- **Architecture Layers**: Often separated into Presentation (UI), Data Content (Logic), Data Representation (In-memory), and Data Storage (Persistence) layers.

## 2. Unique Testing Challenges for Web Apps

Traditional testing techniques (graph, logic, mutation for unit testing) are often difficult to apply directly at the *system level* for web apps due to:

- **Stateless HTTP**: Each request is independent. State must be managed manually (cookies, sessions), making sequences hard to test.

- **Low Controllability**: Inputs are complex HTML forms, making it hard to force specific values into the server-side logic.

- **Low Observability**: Server-side internal state (memory, databases) is usually inaccessible for security reasons.

- **User Interference**: Users can use Back, Forward, Refresh, or edit URLs, disrupting expected flows.

- **Dynamic Content**: Pages are often generated on-demand, making their structure unpredictable.

## 3. Testing Static Web Sites

This is the simplest case, involving websites made of unchanging HTML files.

- **Model**: The site is modeled as a **graph**.
  - **Nodes**: Individual web pages (HTML files).
  - **Edges**: Hyperlinks (`<a href=... >`) between pages.

- **Graph Creation**: Usually done by crawling (like BFS) starting from the home page.

- **Primary Goal**: Find **dead links** (links to invalid URLs).

- **Coverage Criterion**: **Edge Coverage** (traverse every link) is the main goal.

- **Other Tests**: Non-functional tests like load, performance, and access control testing are also important.

## 4. Testing Dynamic Web Apps: Client-Side

This focuses on testing validation done *in the user's browser* before data is sent to the server.

- **Inputs**: HTML form elements (text boxes, radio buttons, dropdowns, etc.).

- **Client-Side Constraints**: Validation rules enforced via:
  - **HTML Attributes**: Common validation attributes include `maxlength`, fixed `value` for radio/select/checkbox, form method (GET/POST).
  - **Client-Side Scripts** (e.g., JavaScript): For more complex checks like data type, format, range, or checks involving multiple fields.

- **Input Value Selection**: Techniques include:
  - **Random generation** (often ineffective)
  - **Using User Session Data** (logs)
  - **Constructing values based on Domain Knowledge** (most common)
  - **Bypass Testing** (for invalid values)

### Bypass Testing

- **The Technique**: Intentionally create inputs that **violate** the client-side constraints.

- **The "Bypass"**: Submit these invalid inputs **directly to the server**, bypassing the client-side validation checks.

- **How**: Save the HTML form locally, **edit it** to remove or disable validation attributes/scripts (e.g., remove `maxlength`, remove `onClick` validation), then submit the modified form.

- **Goal (Test Oracle)**: To see how the **server** handles the unexpected, invalid data.
  - **Pass**: Server correctly identifies invalid data and handles it gracefully (e.g., error message).
  - **Fail**: Server crashes, accepts corrupt data, or exposes abnormal behavior/security vulnerabilities.

- **Example Violations**: Submit strings longer than `maxlength`, values outside allowed ranges, non-numeric data for age, special characters (`<`, `>`, `'`, `&`, `..`, `/`) to test security.

## 5. Testing Dynamic Web Apps: Server-Side (Presentation Layer)

While unit testing server-side code uses standard techniques, system-level testing requires models that capture the dynamic interaction between components. Graph models are created based on the **presentation layer** code.

### I. Atomic Sections

- **Definition**: A section of server-side code that generates a block of HTML with an "all-or-nothing" property: if any part is sent, the whole block is sent. An HTML file itself is an atomic section. They can be empty.

- **Content Variables**: Program variables providing data to an atomic section.

- **Example**:

    ```java
    PrintWriter out = response.getWriter();

    // P1 START
    out.println("<HTML>");
    out.println("<HEAD><TITLE>"+title+"</TITLE></HEAD>");
    out.println("<BODY>");
    // P1 END

    if (User) {
    // P2 START
        out.println("<CENTER> Welcome!</CENTER>");
        
        for (int i=0; i<myVector.size(); i++) {
            if (myVector.elementAt(i).size > 10) {
    // P2 END
    // P3 START
                out.println("<p><b>"+myVector.elementAt(i)+"</b></p>");
    // P3 END
            } else {
    // P4 START
                out.println("<p>"+myVector.elementAt(i)+"</p>");
    // P4 END
            }
        }
    } else
    // P5 START (Empty Atomic Section)
    {
    }
    // P5 END

    // P6 START
    out.println("</BODY></HTML>");
    // P6 END
    out.close();
    ```

- In this example: $P_1$, $P_2$, $P_3$, $P_4$, $P_5$ (empty), $P_6$ are atomic sections
- Content variables: `title`, `User`, `myVector`

### II. Component Expressions

Atomic sections are combined using operators to represent the structure of a dynamically generated page:

- **Sequence**: $P_1 \cdot P_2$ (P1 followed by P2)
- **Selection**: $P_1 \mid P_2$ (Either P1 or P2)
- **Iteration**: $P_1^*$ (Zero or more P1s)
- **Aggregation**: $P_1\{P_2\}$ (P2 is included inside P1)

- **Example Expression**: For the code above: $P_1 \cdot (P_2 \cdot (P_3 \mid P_4)^* \mid P_5) \cdot P_6$

### III. Graph Model 1: Component Interaction Model (CIM)

- **Scope**: **Intra-component** (models behavior *within* a single server-side component, e.g., one servlet).

- **Nodes**: Atomic sections.

- **Edges**: Transitions between atomic sections based on the component expression.

- **Example**: CIM for gradeServlet

    ```mermaid
    graph TD
    P1[P1]
    P2[P2]
    P3[P3]
    P4[P4]
    P5[P5]
    P6[P6]
    
    P1 -.-> P5
    P5 -.-> P6
    
    P1 -.-> P2
    
    P2 -.-> P3
    P2 -.-> P4
    P2 -.-> P6
    
    P3 -.-> P3
    P3 -.-> P4
    P3 -.-> P6
    
    P4 -.-> P4
    P4 -.-> P3
    P4 -.-> P6
    
    style P1 fill:#d946a6,stroke:#333,color:#fff
    style P2 fill:#d946a6,stroke:#333,color:#fff
    style P3 fill:#d946a6,stroke:#333,color:#fff
    style P4 fill:#d946a6,stroke:#333,color:#fff
    style P5 fill:#d946a6,stroke:#333,color:#fff
    style P6 fill:#d946a6,stroke:#333,color:#fff
    ```

This shows the internal flow within the gradeServlet component based on the component expression: $P_1 \cdot (P_2 \cdot (P_3 \mid P_4)^* \mid P_5) \cdot P_6$

### IV. Graph Model 2: Application Transition Graph (ATG)

- **Scope**: **Inter-component** (models flow *between* different components/pages).

- **Nodes**: Each node represents an entire **CIM** (i.e., a whole component or static page).

- **Edges**: Transitions between components. Types include:
  - **Simple Link**: Standard `<a href...>`
  - **Form Link**: Form submission
  - **Component Expression Transition**: Result of server-side code execution
  - **Operational**: User actions outside software control (Back, Forward, Refresh, Edit URL)
  - **Redirect**: Server-side redirect, invisible to user

- **Example**: ATG for gradeServlet

    ```mermaid
    graph TD
        Start([Start])
        Login[login.html]
        Syllabus[syllabus.html]
        SendMail[sendMail]
        
        subgraph gradeServlet ["gradeServlet"]
            direction TB
            P1[P1]
            P2[P2]
            P3[P3]
            P4[P4]
            P5[P5]
            P6[P6]
            
            P1 -.-> P2
            P1 -.-> P4
            P1 -.-> P5
            P2 -.-> P3
            P3 -.-> P3
            P3 -.-> P6
            P4 -.-> P6
            P5 -.-> P6
        end
        
        Start -->|get| Login
        Login -->|get| Syllabus
        Login -->|get Id,Password,retry| P1
        P4 -->|get| SendMail
        P4 -->|get Id,Password,retry| P1
        
        style Login fill:#4a90e2,stroke:#333,color:#fff
        style Syllabus fill:#4a90e2,stroke:#333,color:#fff
        style SendMail fill:#4a90e2,stroke:#333,color:#fff
        style gradeServlet fill:#5a4,stroke:#333,stroke-width:3px
        style P1 fill:#d946a6,stroke:#333,color:#fff
        style P2 fill:#d946a6,stroke:#333,color:#fff
        style P3 fill:#d946a6,stroke:#333,color:#fff
        style P4 fill:#d946a6,stroke:#333,color:#fff
        style P5 fill:#d946a6,stroke:#333,color:#fff
        style P6 fill:#d946a6,stroke:#333,color:#fff
    ```

- **Nodes**: `login.html`, `gradeServlet` (contains CIM with P1-P6), `sendMail`, `syllabus.html`
- **Edges**: Solid arrows show transitions between components, dashed arrows show internal CIM transitions
- Edge labels show HTTP method and parameters (e.g., `get(Id, Password, retry)`)

### V. Benefits & Limitations

- **Benefit**: CIM/ATG model system-level behavior, including dynamic pages and operational transitions.

- **Limitation**: Graphs often need **manual analysis** of source code to generate, especially the ATG. Data flow testing at this level is difficult.
