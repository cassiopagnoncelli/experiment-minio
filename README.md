# Minio

This repository is an example usage of Minio server, an S3-compatible API integrated with Active
Storage.

## Usage

Spin up the server with

```sh
export MINIO_SECRET_ACCESS_KEY=password
bin/rails s -p 3000
```

Go to the console to upload and download the file

```ruby
u = User.create!(name: "User")

# Make sure TODO is a non-empty file available at Rails root.
u.todo.attach(io: File.open("TODO"), filename: "todo")

# Prepend to your server address.
Rails.application.routes.url_helpers.rails_storage_proxy_url(u.todo, only_path:true)
urn = Rails.application.routes.url_helpers.rails_storage_redirect_url(u.todo, only_path:true)

"http://localhost:3000/#{urn}"
```

## Further reference

See this [Medium article](https://medium.com/@drachevskii/rails-7-minio-fast-setup-cheat-sheet-a8e96a8cf3e0).

- `Aws::S3::Errors::NoSuchBucket: The specified bucket does not exist` — You need to create a
bucket you trying to connect.

- `SignatureDoesNotMatch` — some trouble with authorization. Double-check your credentials and
other settings.

- `NetworkingError: Failed to open TCP connection` — may be you provided wrong endpoing or try
to enable/disable `force_path_style` field.
