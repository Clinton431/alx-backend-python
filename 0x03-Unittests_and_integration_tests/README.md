# Unittests and Integration Tests

## Overview

This repository contains a comprehensive testing framework for both unit tests and integration tests. These testing suites are designed to ensure code quality, catch regressions early, and validate that components work correctly both in isolation and together.

## Table of Contents

- [Getting Started](#getting-started)
- [Unit Tests](#unit-tests)
- [Integration Tests](#integration-tests)
- [Test Configuration](#test-configuration)
- [Running Tests](#running-tests)
- [Writing New Tests](#writing-new-tests)
- [Best Practices](#best-practices)
- [Continuous Integration](#continuous-integration)
- [Troubleshooting](#troubleshooting)

## Getting Started

### Prerequisites

- Node.js v18.x or higher
- npm v9.x or higher
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/your-project.git

# Navigate to the project directory
cd your-project

# Install dependencies
npm install
```

## Unit Tests

Unit tests focus on testing individual components in isolation to verify that each part works correctly on its own.

### Structure

Unit tests are located in the `tests/unit/` directory with a structure that mirrors the source code.

### Technologies

- Jest for test runner and assertions
- Mock libraries for isolating dependencies

## Integration Tests

Integration tests verify that different components work together correctly, focusing on the interactions between modules.

### Structure

Integration tests are located in the `tests/integration/` directory.

### Technologies

- Jest for test orchestration
- Supertest for API testing
- TestContainers for database integration tests

## Test Configuration

Configuration files are located in the `config/` directory:

- `jest.config.js` - Main Jest configuration
- `jest.unit.config.js` - Unit test specific settings
- `jest.integration.config.js` - Integration test specific settings

## Running Tests

```bash
# Run all tests
npm test

# Run only unit tests
npm run test:unit

# Run only integration tests
npm run test:integration

# Run tests with coverage report
npm run test:coverage

# Run specific test file
npm test -- path/to/test-file.test.js
```

## Writing New Tests

### Unit Test Example

```javascript
// tests/unit/utils/calculator.test.js
const { add, subtract } = require('../../../src/utils/calculator');

describe('Calculator', () => {
  describe('add function', () => {
    it('should add two positive numbers correctly', () => {
      expect(add(2, 3)).toBe(5);
    });
    
    it('should handle negative numbers', () => {
      expect(add(-2, 3)).toBe(1);
    });
  });
  
  describe('subtract function', () => {
    it('should subtract two numbers correctly', () => {
      expect(subtract(5, 3)).toBe(2);
    });
  });
});
```

### Integration Test Example

```javascript
// tests/integration/api/users.test.js
const request = require('supertest');
const app = require('../../../src/app');
const db = require('../../../src/database');

describe('User API', () => {
  beforeAll(async () => {
    await db.connect();
  });
  
  afterAll(async () => {
    await db.disconnect();
  });
  
  beforeEach(async () => {
    await db.clearCollection('users');
  });
  
  it('should create a new user', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({
        name: 'Test User',
        email: 'test@example.com'
      });
      
    expect(response.status).toBe(201);
    expect(response.body).toHaveProperty('id');
  });
});
```

## Best Practices

1. **Test Isolation**: Each test should be independent and not rely on the state of other tests.
2. **Descriptive Names**: Give your tests descriptive names that explain what they're testing.
3. **Arrange-Act-Assert**: Structure your tests with clear setup, execution, and verification phases.
4. **Mock External Dependencies**: Use mocks for external services to make tests reliable and fast.
5. **Test Edge Cases**: Include tests for boundary conditions and error scenarios.
6. **Keep Tests Fast**: Optimize tests to run quickly to support rapid development.

## Continuous Integration

Tests are automatically run on every pull request and push to the main branch using GitHub Actions. Configuration can be found in `.github/workflows/tests.yml`.

## Troubleshooting

### Common Issues

- **Timeouts**: If integration tests are timing out, check for resource contention or slow external dependencies.
- **Flaky Tests**: If tests pass intermittently, they may have hidden dependencies or race conditions.
- **Memory Issues**: For large test suites, you may need to increase Node.js memory with `NODE_OPTIONS=--max_old_space_size=4096`.

### Debug Mode

```bash
# Run tests in debug mode
npm run test:debug
```

---

For additional support, please open an issue in the repository or contact the development team.
