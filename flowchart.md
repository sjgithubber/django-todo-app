flowchart TB
    subgraph client["Client Browser"]
        request["HTTP Request"]
        response["HTTP Response"]
    end

    subgraph django["Django Project"]
        subgraph wsgi["WSGI Server"]
            wsgi_handler["WSGI Handler"]
        end

        subgraph url_resolver["URL Resolver"]
            urls["urls.py Pattern Matching"]
        end

        subgraph middleware["Middleware Stack"]
            request_middleware["Request Middleware"]
            response_middleware["Response Middleware"]
        end

        subgraph view["View Layer"]
            function_view["Function-based View\n(or Class-based View)"]
            view_logic["Process Request\nValidate Input\nApply Business Logic"]
        end

        subgraph model["Model Layer"]
            model_manager["Model Manager\n(objects)"]
            orm["ORM Operations\n(create, read, update, delete)"]
            db_query["SQL Query Generation"]
        end

        subgraph template["Template Layer"]
            template_engine["Template Engine"]
            template_context["Context Processing"]
            template_rendering["HTML Rendering"]
        end

        subgraph database["Database"]
            db["PostgreSQL/MySQL/SQLite"]
        end
    end

    %% Flow connections
    request --> wsgi_handler
    wsgi_handler --> request_middleware
    request_middleware --> urls
    
    urls --> function_view
    function_view --> view_logic
    
    view_logic --> model_manager
    model_manager --> orm
    orm --> db_query
    db_query --> db
    db --> orm
    
    view_logic --> template_context
    template_context --> template_engine
    template_engine --> template_rendering
    
    template_rendering --> response_middleware
    response_middleware --> response
    
    %% Add example operations for blog app
    subgraph example["Blog App Example (post_detail)"]
        ex1["1. User requests /blog/post/1/"]
        ex2["2. urls.py matches to post_detail view"]
        ex3["3. View calls Post.objects.get(pk=1)"]
        ex4["4. ORM translates to SQL: SELECT * FROM blog_post WHERE id=1"]
        ex5["5. Database returns post data"]
        ex6["6. View renders blog/post_detail.html with post data"]
        ex7["7. Response sent to browser"]
    end
    
    ex1 --> ex2 --> ex3 --> ex4 --> ex5 --> ex6 --> ex7
