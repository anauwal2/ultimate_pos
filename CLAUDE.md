# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AkbarStudios POS V6.7 - A comprehensive Laravel-based Point of Sale and Stock Management System with modular architecture supporting retail, restaurant, manufacturing, and service businesses.

## Technology Stack

- **Framework**: Laravel 9.x (PHP 8.0+)
- **Database**: MySQL
- **Frontend**: Bootstrap 3, jQuery, DataTables
- **API**: Laravel Passport (OAuth2)
- **Real-time**: Pusher
- **Payment**: Multiple gateways (Stripe, PayPal, Razorpay, etc.)

## Essential Development Commands

### Initial Setup
```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan db:seed
php artisan passport:install
php artisan storage:link
```

### Development
```bash
# Clear all caches
php artisan optimize:clear

# Clear specific caches
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear

# Run tests
php artisan test

# Create dummy business data (dev only - deletes existing data!)
php artisan pos:dummyBusiness
```

### Custom Artisan Commands
- `php artisan pos:generateSubscriptionInvoices` - Generate recurring invoices
- `php artisan pos:generateRecurringExpense` - Create recurring expenses
- `php artisan pos:autoSendPaymentReminder` - Send payment reminders
- `php artisan pos:updateRewardPoints` - Update customer rewards

### Module Management
```bash
php artisan module:list
php artisan module:enable [module-name]
php artisan module:disable [module-name]
```

## Architecture & Code Structure

### Modular System
The application uses `nwidart/laravel-modules` with 22 modules including:
- Core: Essentials, Accounting, Asset Management
- Business: CRM, Ecommerce, Manufacturing, Project
- Specialized: Restaurant/HMS, Gym, Field Force
- Integration: WooCommerce, Connector, AI Assistance

Each module follows Laravel conventions within its own namespace.

### Key Patterns
1. **Multi-tenancy**: Supports multiple businesses/locations per installation
2. **Role-based Access**: Uses Spatie Laravel Permission
3. **API-First**: All features accessible via API (Laravel Passport)
4. **Event-Driven**: Extensive use of Laravel events/listeners
5. **Repository Pattern**: Common in larger modules

### Important Locations
- `/app/Http/Controllers` - Main controllers (organized by feature)
- `/app/Models` - Eloquent models with extensive relationships
- `/Modules/*/` - Module-specific code
- `/resources/views` - Blade templates (Bootstrap 3)
- `/database/migrations` - 200+ migration files

### Custom Blade Directives
- `@num_format($number)` - Format numbers
- `@format_quantity($quantity)` - Format quantities
- `@format_currency($amount)` - Format currency
- `@format_date($date)` - Format dates
- `@format_datetime($datetime)` - Format datetime

### Key Models & Relationships
- `Business` - Central model, all data scoped to business
- `User` → `Business` (many-to-many)
- `Transaction` - Handles all financial transactions (polymorphic)
- `Product` → `Variation` → `ProductVariation` (complex inventory)
- `Contact` - Customers and suppliers (type-based)

### Performance Considerations
- Application sets unlimited memory/execution time
- Extensive caching implemented
- Database queries optimized with eager loading
- Background jobs for heavy operations

### Security Notes
- All queries automatically scoped to active business
- API authentication via Laravel Passport
- Activity logging on sensitive operations
- Role-based permissions at method level

## Testing Approach
- PHPUnit configured for Unit and Feature tests
- Test database migrations run automatically
- Use `php artisan test` to run all tests
- Tests located in `/tests/Unit` and `/tests/Feature`

## Common Development Tasks

### Adding New Features
1. Check if feature belongs in existing module or needs new module
2. Follow existing naming conventions and patterns
3. Ensure multi-business compatibility
4. Add appropriate permissions
5. Include API endpoints if applicable

### Debugging Issues
1. Check Laravel logs: `/storage/logs/laravel.log`
2. Enable debug bar in `.env`: `DEBUGBAR_ENABLED=true`
3. Check browser console for JavaScript errors
4. Verify permissions for the user role

### Database Operations
- Always use migrations, never modify database directly
- Scope queries to business: `->where('business_id', $business_id)`
- Use transactions for financial operations
- Follow existing table naming conventions