<div class="container">
      <!-- Navigation moved to top -->
      <nav>
        <ul>
          <li><a href="#home">HOME</a></li>
          <li><a href="#about">ABOUT</a></li>
          <li><a href="#blog">BLOG</a></li>
          <li><a href="#interview">PAST INTERVIEW QUESTIONS</a></li>
        </ul>
      </nav>

      <section id="home">
        <header>
          <h1><strong>Hello World</strong></h1>
          <p>
            Thank you for making this far into this website. It really means the
            world to me that you came by here to visit this little website of
            mine.
          </p>
          <p>
            I, Ahmad Faizal, am building this website and/or blog to showcase my
            skills and perhaps push and remind myself of how much I have
            progressed and improve over the years. I sure do need to improve my
            typing skills. Would be useful in writing codes in the dark of the
            mind.
          </p>
          <p>
            A certain.. website written by
            <a href="https://justinjackson.ca/words.html"> Justin Jackson </a>
            stated that eveything comes with one thing and one thing only:
            <i>words</i>. I like that thinking and it is time I start putting my
            thoughts and words into this space of mine that I am
          </p>
        </header>
        <section aria-labelledby="accessible-semantic-html">
          <h2 id="accessible-semantic-html">Why this?</h2>
        </section>
      </section>

      <section id="about">
        <h2>About</h2>
        <p>
          Welcome to my portfolio! I'm a DevOps professional passionate about
          cloud technologies, automation, and continuous integration. This site
          showcases my journey in mastering Azure DevOps and related
          technologies.
        </p>
        <p>
          Through hands-on projects and real-world implementations, I've
          developed expertise in CI/CD pipelines, infrastructure as code,
          containerization, and cloud architecture.
        </p>
      </section>

      <section id="blog">
        <h2>Blog</h2>

        <article>
          <h3>Title 1</h3>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim
            ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut
            aliquip ex ea commodo consequat. Duis aute irure dolor in
            reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
            pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
            culpa qui officia deserunt mollit anim id est laborum.
          </p>
          <p>
            Sed ut perspiciatis unde omnis iste natus error sit voluptatem
            accusantium doloremque laudantium, totam rem aperiam, eaque ipsa
            quae ab illo inventore veritatis et quasi architecto beatae vitae
            dicta sunt explicabo.
          </p>
        </article>
        <article>
          <h3>Title 2</h3>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris vel
            magna in nunc tempor bibendum. Vestibulum ante ipsum primis in
            faucibus orci luctus et ultrices posuere cubilia curae; Nullam quis
            risus eget urna mollis ornare vel eu leo. Cras mattis consectetur
            purus sit amet fermentum.
          </p>
          <p>
            Donec sed odio dui. Etiam porta sem malesuada magna mollis euismod.
            Nullam id dolor id nibh ultricies vehicula ut id elit. Morbi leo
            risus, porta ac consectetur ac, vestibulum at eros. Praesent commodo
            cursus magna, vel scelerisque nisl consectetur et.
          </p>
        </article>

        <article>
          <h3>Title 3</h3>
          <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce
            dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh,
            ut fermentum massa justo sit amet risus. Sed posuere consectetur est
            at lobortis. Cras justo odio, dapibus ac facilisis in, egestas eget
            quam.
          </p>
          <p>
            Aenean lacinia bibendum nulla sed consectetur. Cras mattis
            consectetur purus sit amet fermentum. Vivamus sagittis lacus vel
            augue laoreet rutrum faucibus dolor auctor. Maecenas sed diam eget
            risus varius blandit sit amet non magna.
          </p>
        </article>
      </section>

      <!--
      1. A new section to preserve all of my interview questions
      2. This will be used to pull the API for all contentsS
     -->

      <section id="interview">
        <h2>Past Interview Questions</h2>

        <article>
          <h3>Title 1</h3>
          <p>
            Q: Can you explain what Express.js is and what role it plays in a
            Node.js application?
          </p>
          <p>
            A: Better answer would be: "Express.js is a minimal and flexible
            Node.js web application framework that provides a robust set of
            features for web and mobile applications. It acts as a layer on top
            of Node.js that simplifies server creation and provides features
            like routing, middleware support, template engines, and
            request/response handling."
          </p>
        </article>

        <article>
          <h3>Title 2</h3>
          <p>
            Q: Now, can you walk me through the basic lifecycle of an Express.js
            request? What happens from when a request hits your server until a
            response is sent back? Note: I'll definitely include scalability and
            coding questions as we progress through the topics!
          </p>
          <p>
            1. Receive the Request Object: either Post, Get, Put, Patch and
            Delete 2. Express listens for the right routes 3. Middleware
            processing: auth, helmet, validation, sanitisation etc 4. Route
            handles the execution 5. Server sends the response 6. Ends the
            process if it is written
          </p>
        </article>

        <article>
          <h3>Title 3</h3>
          <p>
            Q: You mentioned validation in middleware. Can you explain how you
            would implement request validation in Express? Specifically, what's
            your approach to input validation and sanitization, and what tools
            would you use? Also, what is cross-validation in this context? Take
            your time with this one - validation is crucial for security.
          </p>
          <p>
            A: I can see you understand the basic principles, but let me help
            you improve this answer and clarify some concepts: What you got
            right: ✅ Type checking (INT, string validation) ✅ Token validation
            ✅ Basic SQL injection prevention awareness Areas for improvement:
            Tools vs Libraries confusion: Libraries are tools! Popular
            validation libraries include Joi, Yup, express-validator, or Zod.
            Regex for SQL injection: While regex can help, it's not the most
            robust approach. Better methods: Use parameterized queries/prepared
            statements ORM/ODM (Sequelize, Mongoose) that handles escaping Input
            validation libraries that sanitize properly Missing validation
            concepts: Schema validation (defining expected structure)
            Sanitization (cleaning data, not just rejecting) Error handling and
            user feedback Additionally: Deep Dive: Request Validation in
            Express.js Understanding the Foundation of Validation Request
            validation in Express.js is your application's first line of defense
            against malicious data and ensures data integrity throughout your
            system. The core principle involves examining incoming data before
            it reaches your business logic, ensuring it meets predefined
            criteria for format, type, length, and content. This process
            operates on multiple levels: syntactic validation (checking if data
            matches expected patterns), semantic validation (ensuring data makes
            logical sense in your domain), and security validation (preventing
            injection attacks and malicious payloads). Modern validation
            approaches move beyond simple regex checks to embrace schema-based
            validation, where you define the complete structure and constraints
            of expected data upfront, making your validation both more
            comprehensive and maintainable. Implementing Robust Validation
            Strategies The most effective validation implementations combine
            multiple layers of protection using specialized libraries rather
            than custom regex solutions. Libraries like express-validator
            provide chainable validation methods that handle common scenarios:
            body('email').isEmail().normalizeEmail() not only validates email
            format but also normalizes it to prevent case-sensitivity issues.
            Joi offers schema-based validation where you define comprehensive
            data structures: Joi.object({ name:
            Joi.string().min(3).max(50).required(), age:
            Joi.number().integer().min(0).max(120) }). These libraries
            automatically handle edge cases, provide consistent error messaging,
            and offer built-in sanitization. Sanitization is crucial - it's not
            enough to reject invalid data; you should clean and normalize valid
            data (trimming whitespace, converting case, escaping HTML entities).
            Always validate at multiple points: client-side for user experience,
            server-side for security, and database-level for data integrity
            constraints. Advanced Validation Concepts and Security
            Considerations Beyond basic type checking lies sophisticated
            validation patterns that professional applications require.
            Cross-field validation ensures related fields maintain logical
            relationships (e.g., end_date must be after start_date), while
            conditional validation applies different rules based on other field
            values (e.g., if user_type is 'premium', additional fields become
            required). For security, implement rate limiting per endpoint,
            validate file uploads with proper MIME type checking and size
            restrictions, and use allowlists rather than blocklists for
            acceptable values. SQL injection prevention goes beyond regex - use
            parameterized queries exclusively, implement proper ORM usage, and
            validate against your database schema constraints. Consider
            implementing validation middleware that runs before authentication
            to prevent resource waste on malicious requests, but ensure
            sensitive validations occur after authentication to prevent
            information leakage. Error responses should be informative enough
            for legitimate users but not reveal system internals to potential
            attackers - return generic error messages for security-related
            failures while providing detailed feedback for format errors.
          </p>
        </article>
      </section>
    </div>
