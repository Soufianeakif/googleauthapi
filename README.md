
# Laravel Google OAuth API

This is a Laravel API that implements Google OAuth authentication using the Laravel Socialite package. It allows users to log in via their Google account and returns user information such as name, email, and profile picture.



## Installation

1. Clone the repository

```bash
git clone https://github.com/Soufianeakif/googleauthapi
cd googleauthapi
```

2. Install dependencies

```bash
composer install
``` 

3. Set up environment variables

Copy the ```.env.example``` file to ```.env```.
```bash
cp .env.example .env
```

Now, open the .env file and configure your database and Google OAuth credentials.

#### Database Configuration

```bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=your_database_user
DB_PASSWORD=your_database_password
``` 

#### Google OAuth Configuration

```bash
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_REDIRECT_URI=/auth/google/callback
``` 

Make sure you replace the placeholders with your actual credentials.

4. Generate application key
Laravel requires an application key for encryption. Generate it using:

```bash
php artisan key:generate
```

5. Run migrations

```bash
php artisan migrate
```

6. Start the development server

```bash
php artisan serve
```

The server will be running at ```http://127.0.0.1:8000```.



## API Endpoints

1. Google OAuth Redirect
This endpoint redirects the user to Google's OAuth consent page.

Method: GET
URL: ```/auth/google/redirect```

Example:

```bash
GET http://127.0.0.1:8000/auth/google/redirect
```

2. Google OAuth Callback
This endpoint is the callback URL where Google sends the authenticated user's data. After successful login, the user will be created or updated in the database and logged in automatically.
Method: GET
URL: ```/auth/google/callback```

Example:

```bash
GET http://127.0.0.1:8000/auth/google/callback
```

3. Redirect to Dashboard
Upon successful login, the user will be redirected to the frontend dashboard.

Frontend URL: Configurable in .env as APP_FRONTEND_URL.

## Code Snippet

```bash
use Illuminate\Support\Facades\Route;
use Laravel\Socialite\Facades\Socialite;
use Illuminate\Http\Request;
use App\Models\User;
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Str;

Route::get('/auth/google/redirect', function (Request $request) {
    return Socialite::driver("google")->redirect();
});

Route::get('/auth/google/callback', function (Request $request) {
    $googleUser = Socialite::driver("google")->user();

    $user = User::updateOrCreate(
        ['google_id' => $googleUser->id],
        [
            'name' => $googleUser->name,
            'email' => $googleUser->email,
            'image' => $googleUser->avatar,
            'password' => Str::random(12),
        ]
    );

    Auth::login($user);

    return redirect(config("app.frontend_url") . "/dashboard");
});

```
## Screenshots

![App Screenshot](https://i.ibb.co/19YXRXG/apiimg1.jpg)
![App Screenshot](https://i.ibb.co/gt3dGFH/apiimg2.jpg)
![App Screenshot](https://i.ibb.co/QKv1wKn/apiimg3.jpg)


## 🔗 Links
[![portfolio](https://img.shields.io/badge/my_portfolio-000?style=for-the-badge&logo=ko-fi&logoColor=white)](https://akifsoufiane.tech/)
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/soufiane-akif/)


## License

This project is licensed under the MIT License.

