
Testing random things.

## Creating a cheap EC2 instance

1. Get the cheapest `t2.micro` spot price.

    ```sh
    # it might be better to redirect the aws output to a file then jq that, e.g. `aws ec2 describe-spot-price-history --no-paginate > spot-prices.json`
    aws ec2 describe-spot-price-history --instance-types t2.micro --no-paginate | jq "[.SpotPriceHistory | .[]] | sort_by(.SpotPrice) | .[0]"
    ```

1. Now get the list of AMIs.

    ```sh
    aws ec2 describe-images --filters Name=architecture,Values=x86_64
    ```

1. Now that you know where the cheap spot instances are, create the EC2 instance with Terraform.

    1. Grab the Terraform EC2 complete instance example.

        ```sh
        git clone https://github.com/terraform-aws-modules/terraform-aws-ec2-instance
        ```
    
    1. Switch to the complete EC2 instance directory.

        ```sh
        cd terraform-aws-ec2-instance/examples/complete
        ```
    
    1. Set us up the instance.

        ```sh
        terraform init
        terraform plan
        terraform apply
        ```