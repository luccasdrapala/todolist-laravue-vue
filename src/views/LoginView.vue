<template>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
</head>
<body>
    
    <TheHeaderLogin/>
    <LoginBox/>

</body>
</html>
    
</template>

<script>
import LoginBox from '@/components/LoginBox.vue'
import TheHeaderLogin from '@/components/TheHeaderLogin.vue';

    export default{
        components: {
            // TheHeaderLogin,
            LoginBox,
            TheHeaderLogin
        },
    }

</script>