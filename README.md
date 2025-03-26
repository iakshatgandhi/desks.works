# Modern Blog Platform

A feature-rich, responsive blogging platform built with Next.js 13, PostgreSQL, and Cloudinary.

![Blog Platform Screenshot](https://place-hold.it/800x400/0a0a0a/ffffff&text=Blog+Platform&fontsize=30)

## Features

- **Modern UI**: Responsive design with a clean, user-friendly interface
- **Dynamic Blog Layout**: Masonry grid layout that adapts to content length
- **Rich Content Editor**: Support for formatted text, images, and embeds
- **User Authentication**: Secure login, registration, and profile management
- **Image Management**: Upload and manage images with Cloudinary integration
- **Categories**: Organize posts with categories and filtering
- **Comments & Likes**: Engage with readers through comments and likes
- **Dashboard**: Author dashboard for post management
- **SEO Optimized**: Built with SEO best practices in mind

## Tech Stack

- **Frontend**: Next.js 13 (App Router), React, TypeScript, Tailwind CSS
- **Backend**: Next.js API Routes, PostgreSQL, Prisma ORM
- **Authentication**: NextAuth.js
- **Image Storage**: Cloudinary
- **Deployment**: Vercel (or your preferred platform)

## Architecture Overview

The application follows a modern architecture with clear separation of concerns:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│                 │     │                 │     │                 │
│  Next.js App    │────▶│  API Routes     │────▶│  Database       │
│  (React UI)     │     │  (Server-side)  │     │  (PostgreSQL)   │
│                 │     │                 │     │                 │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │                        │
        │                       │                        │
        ▼                       ▼                        ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│                 │     │                 │     │                 │
│  Authentication │     │  Cloudinary     │     │  Prisma ORM     │
│  (NextAuth)     │     │  (Image Store)  │     │  (Data Access)  │
│                 │     │                 │     │                 │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

## Getting Started

### Prerequisites

- Node.js (v18 or later)
- PostgreSQL database
- Cloudinary account
- Git
- A code editor (VS Code recommended)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/blog-platform.git
   cd blog-platform
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Set up environment variables:
   ```bash
   cp .env.example .env
   ```
   
   Then update the `.env` file with your credentials:
   ```
   DATABASE_URL="postgresql://username:password@localhost:5432/blog_db"
   CLOUDINARY_CLOUD_NAME="your_cloud_name"
   CLOUDINARY_API_KEY="your_api_key"
   CLOUDINARY_API_SECRET="your_api_secret"
   NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME="your_cloud_name"
   NEXTAUTH_SECRET="your_generated_secret"
   NEXTAUTH_URL="http://localhost:3000"
   ```

4. Initialize the database:
   ```bash
   npx prisma migrate dev
   npm run seed
   ```

5. Start the development server:
   ```bash
   npm run dev
   ```

6. Open your browser and navigate to [http://localhost:3000](http://localhost:3000)

## Project Structure

```
├── app/
│   ├── api/                     # API routes (App Router)
│   │   ├── posts/               # Post-related endpoints
│   │   ├── upload/              # Image upload endpoint
│   │   └── ...
│   ├── dashboard/               # Dashboard pages
│   ├── post/                    # Post detail pages
│   └── page.tsx                 # Homepage
├── components/
│   ├── ui/                      # UI components
│   │   ├── image-uploader.tsx   # Image upload component
│   │   └── ...
│   ├── blog-card.tsx            # Blog card component
│   ├── main-nav.tsx             # Main navigation component
│   └── ...
├── lib/
│   ├── prisma.ts                # Prisma client
│   ├── utils.ts                 # Utility functions
│   └── ...
├── prisma/
│   └── schema.prisma            # Database schema
├── public/
│   └── ...                      # Static assets
└── scripts/
    └── seed.ts                  # Database seed script
```

## Database Schema

The application uses PostgreSQL with Prisma ORM. Main entities include:

- `User` - User accounts
- `Post` - Blog posts
- `Category` - Post categories
- `Comment` - Post comments
- `Like` - Post likes

See `prisma/schema.prisma` for the complete schema definition.

## API Endpoints

### Authentication

- `POST /api/register` - Register a new user
- `POST /api/login` - User login
- `POST /api/logout` - User logout
- `GET /api/user` - Get current user

### Posts

- `GET /api/posts` - Get all posts
- `GET /api/posts/{id}` - Get a specific post
- `POST /api/posts` - Create a new post
- `PUT /api/posts/{id}` - Update a post
- `DELETE /api/posts/{id}` - Delete a post
- `GET /api/posts/user/{userId}` - Get posts by user

### Comments

- `POST /api/posts/{postId}/comments` - Add a comment
- `DELETE /api/posts/{postId}/comments/{commentId}` - Delete a comment

### Categories

- `GET /api/categories` - Get all categories

### File Upload

- `POST /api/upload` - Upload an image

### Likes

- `POST /api/posts/{postId}/like` - Like a post
- `DELETE /api/posts/{postId}/like` - Unlike a post

For detailed API documentation, see [API Documentation](./API_DOCS.md).

## Development Workflow

### Branch Naming Convention

- `feature/feature-name` - for new features
- `fix/issue-description` - for bug fixes
- `refactor/component-name` - for code refactoring
- `docs/update-description` - for documentation updates

### Commit Message Guidelines

Follow the conventional commit format:
```
<type>(<scope>): <subject>

<body>

<footer>
```

Types:
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code
- `refactor`: A code change that neither fixes a bug nor adds a feature
- `test`: Adding missing tests or correcting existing tests
- `chore`: Changes to the build process or auxiliary tools

### Code Review Process

1. Create a pull request with a clear description of changes
2. Link relevant issues or tickets
3. Ensure all CI checks pass
4. Request reviews from at least one team member
5. Address reviewer comments
6. Merge once approved

## Testing

### Running Tests

```bash
# Run all tests
npm test

# Run specific test file
npm test -- components/MyComponent.test.tsx

# Watch mode
npm test -- --watch
```

### Writing Tests

Example of a component test:

```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { MyComponent } from './MyComponent';

describe('MyComponent', () => {
  it('renders correctly', () => {
    render(<MyComponent prop1="test" prop2={42} />);
    expect(screen.getByText('test')).toBeInTheDocument();
  });
  
  it('handles click events', () => {
    const mockFn = jest.fn();
    render(<MyComponent prop1="test" prop2={42} onClick={mockFn} />);
    fireEvent.click(screen.getByText('Click me'));
    expect(mockFn).toHaveBeenCalled();
  });
});
```

## Performance Optimization

### Frontend

- Use React.memo for components that render often but rarely change
- Implement virtualization for long lists
- Optimize images with Next.js Image component
- Use code splitting with dynamic imports

### Backend

- Use pagination for large datasets
- Add database indexes for frequently queried fields
- Cache expensive operations
- Optimize database queries

## Security Considerations

1. **API Authentication**: Protected routes require valid JWT tokens
2. **CSRF Protection**: Built-in CSRF protection for API routes
3. **Input Validation**: All user inputs are validated before processing
4. **Password Handling**: Passwords are hashed using bcrypt
5. **Rate Limiting**: API endpoints have rate limiting to prevent abuse

## Deployment

### Preview Deployments

Every pull request automatically creates a preview deployment.

### Production Deployment

1. Merge changes to the main branch
2. CI/CD pipeline will automatically deploy to production
3. Verify the deployment in the production environment

## Troubleshooting

### Common Issues

1. **Database connection issues**
   - Check your DATABASE_URL in .env
   - Ensure PostgreSQL service is running

2. **Prisma client generation errors**
   - Run `npx prisma generate` to update the client

3. **Next.js build errors**
   - Ensure all imports are correct
   - Check for type errors: `npm run type-check`

4. **Authentication issues**
   - Verify NEXTAUTH_SECRET in .env
   - Check for expired tokens

### Getting Help

- Check existing GitHub issues
- Ask in the team Slack channel: #blog-app-dev
- Document solutions for recurring issues in the wiki

## Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [Prisma Documentation](https://www.prisma.io/docs)
- [Cloudinary Documentation](https://cloudinary.com/documentation)
- [Project Wiki](https://your-project-wiki-url)

## Demo Account

After running the seed script, you can log in with the following demo account:

- **Email**: demo@example.com
- **Password**: password123

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgements

- [Next.js](https://nextjs.org/)
- [Prisma](https://www.prisma.io/)
- [Cloudinary](https://cloudinary.com/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Lucide Icons](https://lucide.dev/)

---

Made with ❤️ by Akshat Gandhi 