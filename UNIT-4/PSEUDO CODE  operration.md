                class RelationalQueryExecutor {

    // Method to execute the relational query
    public static ResultSet executeRelationalQuery(String query) {
        // Step 1: Parse the SQL query into a syntax tree
        SyntaxTree syntaxTree = parseSQL(query);
        if (syntaxTree.hasErrors()) {
            throw new QueryException("Syntax Error in SQL Query");
        }

        // Step 2: Validate the query
        ValidationResult validationResult = validateQuery(syntaxTree);
        if (validationResult.hasErrors()) {
            throw new QueryException("Query Validation Failed");
        }

        // Step 3: Convert to relational algebra operations
        RelationalAlgebraTree relationalAlgebraTree = convertToRelationalAlgebra(syntaxTree);

        // Step 4: Query Optimization
        QueryPlan optimizedPlan = optimizeQueryPlan(relationalAlgebraTree);

        // Step 5: Execute the optimized plan
        ResultSet resultSet = new ResultSet();
        for each QueryOperation operation in optimizedPlan.getOperations() {
            executeOperation(operation, resultSet);
        }

        // Step 6: Assemble the final result set and return
        return resultSet;
    }

    // Step 1: Parse the SQL query into a syntax tree
    private static SyntaxTree parseSQL(String query) {
        // Convert SQL query into syntax tree representation
        return new SyntaxTree(query);
    }

    // Step 2: Validate the parsed query
    private static ValidationResult validateQuery(SyntaxTree syntaxTree) {
        // Validate if the tables, columns, and conditions in the query are correct
        return new ValidationResult(true); // Assume valid for simplicity
    }

    // Step 3: Convert the syntax tree to relational algebra
    private static RelationalAlgebraTree convertToRelationalAlgebra(SyntaxTree syntaxTree) {
        // Convert SQL syntax tree to relational algebra operations (select, project, join, etc.)
        return new RelationalAlgebraTree();
    }

    // Step 4: Optimize the query plan
    private static QueryPlan optimizeQueryPlan(RelationalAlgebraTree relationalAlgebraTree) {
        // Apply optimization techniques to the query plan (e.g., join order optimization)
        return new QueryPlan();
    }

    // Step 5: Execute the query operations
    private static void executeOperation(QueryOperation operation, ResultSet resultSet) {
        switch (operation.getType()) {
            case TABLE_SCAN:
                scanTable(operation, resultSet);
                break;
            case SELECTION:
                applySelection(operation, resultSet);
                break;
            case PROJECTION:
                applyProjection(operation, resultSet);
                break;
            case JOIN:
                performJoin(operation, resultSet);
                break;
            case AGGREGATION:
                performAggregation(operation, resultSet);
                break;
            case SORT:
                performSort(operation, resultSet);
                break;
            default:
                throw new UnsupportedOperationException("Unsupported operation: " + operation.getType());
        }
    }

    // Perform a table scan operation
    private static void scanTable(QueryOperation operation, ResultSet resultSet) {
        // Implement logic for table scan
    }

    // Apply selection operation (filtering)
    private static void applySelection(QueryOperation operation, ResultSet resultSet) {
        // Implement filtering logic based on the WHERE clause
    }

    // Apply projection operation (column extraction)
    private static void applyProjection(QueryOperation operation, ResultSet resultSet) {
        // Implement projection logic (extract specific columns)
    }

    // Perform join operation
    private static void performJoin(QueryOperation operation, ResultSet resultSet) {
        // Implement join logic (INNER JOIN, LEFT JOIN, etc.)
    }

    // Perform aggregation operation (SUM, COUNT, AVG, etc.)
    private static void performAggregation(QueryOperation operation, ResultSet resultSet) {
        // Implement aggregation logic here
    }

    // Perform sorting operation
    private static void performSort(QueryOperation operation, ResultSet resultSet) {
        // Implement sorting based on ORDER BY clause
    }

    // Main method to run the code
    public static void main(String[] args) {
        String sqlQuery = "SELECT employee.name, department.name " +
                          "FROM employee JOIN department " +
                          "ON employee.dept_id = department.id " +
                          "WHERE employee.salary > 50000";

        try {
            ResultSet result = executeRelationalQuery(sqlQuery);
            // Display or process the result set
        } catch (QueryException e) {
            e.printStackTrace();
        }
    }
}
