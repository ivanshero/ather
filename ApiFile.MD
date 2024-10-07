### تعليمات واقتراحات بناءً على المحادثة:

#### 1. **إعداد GraphQL Client**:

- تأكد من إعداد GraphQL Client بشكل صحيح في **Flutter** باستخدام مكتبة **graphql_flutter**.
- رابط الـ API سيكون عبر `https://makesbooks.com/api/graphql`.
- تأكد من وجود قيمة **session** أو **token** عند تسجيل الدخول، حيث ستحتاج إلى استخدامه للمصادقة وتنفيذ الاستعلامات بشكل آمن.

#### 2. **إنشاء الحساب وتسجيل الدخول**:

- تأكد من إنشاء **Mutation** لإنشاء حساب جديد مع الحقول المطلوبة مثل **name**، **email**، و**password**.
- عند تنفيذ استعلام تسجيل الدخول، تأكد من التحقق من حالة المستخدم المحظور (isBlocked) وإظهار رسائل مناسبة للمستخدم إذا تم حظره.

#### 3. **التحقق من المستخدم المحظور**:

- استخدم الاستعلام الخاص بـ **`isUserBlocked`** في جميع الشاشات الحساسة مثل الملفات الشخصية أو الدردشات.
- يمكن إضافة حالة التحقق من الحظر في الشاشة الرئيسية لتجنب مشاكل لاحقة عند محاولة المستخدم الوصول إلى البيانات.

#### 4. **تخصيص الواجهة بناءً على الصلاحيات**:

- إذا كان المستخدم يمتلك صلاحيات **Admin** أو **SuperAdmin**، امنحه القدرة على الوصول إلى الأدوات الإدارية مثل تعديل المحتوى أو إدارة المستخدمين.
- يمكنك التحكم في ظهور مكونات معينة في الواجهة اعتمادًا على دور المستخدم. على سبيل المثال، يمكن إخفاء أو إظهار مكونات مثل "إضافة مقال" أو "إدارة المستخدمين" بناءً على صلاحيات المستخدم.

#### 5. **التعامل مع الأذونات في التطبيق**:

- استخدم **`isAdminOrSuperAdmin`** أو **`isAdminSuperAdminOrCurrentUser`** للتحقق من الأذونات عند تنفيذ العمليات الحساسة مثل تعديل البيانات.
- احرص على التحقق من الصلاحيات لكل عملية على مستوى المستخدم، لتفادي الوصول غير المصرح به.
- يمكن استخدام الاستعلامات لعرض صلاحيات المستخدم في ملفه الشخصي وتحديث الواجهة بشكل ديناميكي بناءً على صلاحياته.

#### 6. **التعامل مع الإشعارات**:

- استعلامات وإدخالات الإشعارات يجب أن يتم التعامل معها بشكل مرتب، مع عرض الإشعارات بشكل صحيح بناءً على ترتيب **notificationNumber**.
- إذا كان هناك نظام إشعارات في التطبيق، تأكد من توفير طريقة سلسة لإدارة الإشعارات وربطها بحسابات المستخدمين.

#### 7. **التكامل مع الجلسة (Session Integration)**:

- تأكد من التعامل مع الجلسة بشكل صحيح بعد تسجيل الدخول، سواء باستخدام **LocalStorage** أو **SecureStorage** لحفظ **Token** أو **Session** في التطبيق.
- احرص على تأمين الجلسة عبر استخدام `Bearer Token` أو **Session ID** لكل استعلام أو عملية تُجرى عبر التطبيق.

#### 8. **تحسين تجربة المستخدم**:

- تأكد من إظهار رسائل واضحة عند حدوث أخطاء، مثل فشل تسجيل الدخول أو إنشاء الحساب أو التحقق من الحظر.
- يمكن تحسين تجربة المستخدم عن طريق توفير أدوات مثل رسائل التنبيه والإشعارات الفورية عند التعامل مع بياناتهم.

#### 9. **إدارة البيانات الحساسة**:

- تأكد من إخفاء الحقول الحساسة مثل كلمات المرور ومفاتيح API الخاصة بالـ Telegram و YouTube و Twitter عن المستخدمين الآخرين.
- يمكن إضافة آليات للمصادقة الثنائية (2FA) في المستقبل لتحسين أمان الحسابات.

#### 10. **الصيانة المستقبلية**:

- يمكنك توثيق الكود وإضافة تعليقات توضيحية في جميع أجزاء الكود لسهولة صيانته من قبل المطورين الآخرين.
- تأكد من تحديث نظام الصلاحيات والبيانات وفقًا لاحتياجات المشروع المستقبلية.

---

### **تسجيل الدخول وإنشاء حساب باستخدام Flutter و GraphQL :**

#### 1. **إعداد GraphQL Queries & Mutations**:

ستحتاج إلى كتابة **mutation** لإنشاء حساب مستخدم جديد و **query** للتحقق من بيانات تسجيل الدخول.

#### **Mutation لإنشاء حساب جديد:**

```graphql
mutation createUser($name: String!, $email: String!, $password: String!) {
  createUser(data: {
    name: $name,
    email: $email,
    password: $password
  }) {
    id
    name
    email
  }
}
```

#### **Query لتسجيل الدخول (التحقق من بيانات المستخدم):**

```graphql
query loginUser($email: String!, $password: String!) {
  authenticateUserWithPassword(email: $email, password: $password) {
    ... on UserAuthenticationWithPasswordSuccess {
      item {
        id
        name
        email
      }
    }
    ... on UserAuthenticationWithPasswordFailure {
      message
    }
  }
}
```

---

### **2. تنفيذ تسجيل الدخول وإنشاء الحساب في Flutter**:

#### **إعداد GraphQL Client في Flutter**:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: AuthenticationScreen(),
      ),
    );
  }
}
```

#### **شاشة تسجيل الدخول وإنشاء الحساب**:

```dart
class AuthenticationScreen extends StatefulWidget {
  @override
  _AuthenticationScreenState createState() => _AuthenticationScreenState();
}

class _AuthenticationScreenState extends State<AuthenticationScreen> {
  bool isLogin = true; // للتحكم بين شاشة تسجيل الدخول وإنشاء الحساب
  final TextEditingController nameController = TextEditingController();
  final TextEditingController emailController = TextEditingController();
  final TextEditingController passwordController = TextEditingController();

  final String createUserMutation = """
    mutation createUser(\$name: String!, \$email: String!, \$password: String!) {
      createUser(data: {name: \$name, email: \$email, password: \$password}) {
        id
        name
        email
      }
    }
  """;

  final String loginUserQuery = """
    query loginUser(\$email: String!, \$password: String!) {
      authenticateUserWithPassword(email: \$email, password: \$password) {
        ... on UserAuthenticationWithPasswordSuccess {
          item {
            id
            name
            email
          }
        }
        ... on UserAuthenticationWithPasswordFailure {
          message
        }
      }
    }
  """;

  void toggleForm() {
    setState(() {
      isLogin = !isLogin;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(isLogin ? "Login" : "Create Account"),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            if (!isLogin)
              TextField(
                controller: nameController,
                decoration: InputDecoration(labelText: 'Name'),
              ),
            TextField(
              controller: emailController,
              decoration: InputDecoration(labelText: 'Email'),
            ),
            TextField(
              controller: passwordController,
              decoration: InputDecoration(labelText: 'Password'),
              obscureText: true,
            ),
            SizedBox(height: 20),
            Mutation(
              options: MutationOptions(
                document: gql(isLogin ? loginUserQuery : createUserMutation),
              ),
              builder: (RunMutation runMutation, QueryResult? result) {
                if (result != null && result.hasException) {
                  return Text("Error: ${result.exception.toString()}");
                }
                if (result != null && result.isLoading) {
                  return CircularProgressIndicator();
                }

                return ElevatedButton(
                  onPressed: () {
                    if (isLogin) {
                      runMutation({
                        'email': emailController.text,
                        'password': passwordController.text,
                      });
                    } else {
                      runMutation({
                        'name': nameController.text,
                        'email': emailController.text,
                        'password': passwordController.text,
                      });
                    }
                  },
                  child: Text(isLogin ? "Login" : "Create Account"),
                );
              },
            ),
            TextButton(
              onPressed: toggleForm,
              child: Text(isLogin
                  ? "Don't have an account? Sign up"
                  : "Already have an account? Log in"),
            ),
          ],
        ),
      ),
    );
  }
}
```

### **شرح الكود**:

1. **شاشة تسجيل الدخول وإنشاء الحساب**:

   - تحتوي على **TextField** لإدخال البيانات مثل **الاسم، البريد الإلكتروني، وكلمة المرور**.
   - يمكن التبديل بين **شاشة تسجيل الدخول** وشاشة **إنشاء الحساب** عبر الزر السفلي.
2. **تسجيل الدخول**:

   - يتم استخدام **GraphQL Query** للتحقق من بيانات المستخدم (البريد الإلكتروني وكلمة المرور).
   - إذا كانت البيانات صحيحة، يتم جلب **المستخدم** وتسجيل الدخول.
   - إذا كانت البيانات غير صحيحة، يتم عرض رسالة خطأ.
3. **إنشاء الحساب**:

   - يتم استخدام **GraphQL Mutation** لإنشاء حساب مستخدم جديد.
   - بعد إدخال البيانات، يتم إرسالها إلى API لإنشاء المستخدم.

---

### **ملخص العملية**:

- **تسجيل الدخول**: التحقق من بيانات المستخدم عبر GraphQL Query.
- **إنشاء حساب جديد**: إنشاء مستخدم جديد عبر GraphQL Mutation.
- **واجهة Flutter**: عرض الحقول وتبديل بين شاشة تسجيل الدخول وإنشاء الحساب مع استخدام **graphql_flutter** لتنفيذ الاستعلامات والمتحولات.

---

### **قسم الأذونات وكيفية عمله مع GraphQL**

---

#### مفهوم الأذونات

الأذونات تتحكم في من يمكنه الوصول إلى الموارد والبيانات المختلفة في النظام. يعتمد هذا القسم على مجموعة من الوظائف التي تتحقق من صلاحيات المستخدم بناءً على عدة عوامل، مثل:

- **دور المستخدم**: مثل Admin أو SuperAdmin.
- **حالة المستخدم**: مثل ما إذا كان المستخدم محظورًا أو لا.
- **التحقق مما إذا كان المستخدم هو نفسه الذي يحاول الوصول إلى بياناته**.

#### الوظائف المستخدمة للتحقق من الأذونات:

1. **`allowAll`**:

   - هذه الوظيفة تسمح للجميع بالوصول إلى البيانات دون قيود.
   - **الاستخدام**: يتم استخدامها عندما نريد أن يكون الوصول مفتوحًا للجميع، مثل قراءة المقالات أو عرض الصفحات العامة.

   ```graphql
   query {
     allArticles {
       title
       content
     }
   }
   ```
2. **`isLoggedIn`**:

   - يتحقق مما إذا كان المستخدم مسجلًا الدخول.
   - يتضمن أيضًا التحقق مما إذا كان المستخدم محظورًا من خلال وظيفة **`isUserBlocked`**.
   - **الاستخدام**: يُستخدم في الأقسام التي تتطلب أن يكون المستخدم قد قام بتسجيل الدخول، مثل إنشاء أو تعديل المقالات.

   ```graphql
   query {
     currentUser {
       name
       email
     }
   }
   ```
3. **`isSuperAdmin`**:

   - يتحقق مما إذا كان المستخدم من نوع **SuperAdmin**.
   - **الاستخدام**: يتم استخدامه لتقييد العمليات التي يمكن فقط للمسؤولين الكبار الوصول إليها، مثل حذف المستخدمين أو تعديل الإعدادات الحساسة.

   ```graphql
   mutation {
     deleteUser(id: "1") {
       id
       name
     }
   }
   ```
4. **`isAdminOrSuperAdmin`**:

   - يتحقق مما إذا كان المستخدم من نوع **Admin** أو **SuperAdmin**.
   - **الاستخدام**: يسمح للمسؤولين بإدارة البيانات مثل تعديل المحتوى أو إدارة المستخدمين الآخرين.

   ```graphql
   mutation {
     updateArticle(id: "1", data: { title: "New Title" }) {
       id
       title
     }
   }
   ```
5. **`isCurrentUser`**:

   - يتحقق مما إذا كان المستخدم هو نفسه الذي يحاول تعديل بياناته.
   - **الاستخدام**: يستخدم لتقييد تعديل البيانات الشخصية للمستخدم نفسه فقط.

   ```graphql
   mutation {
     updateUser(id: "1", data: { name: "New Name" }) {
       id
       name
     }
   }
   ```
6. **`isAdminSuperAdminOrCurrentUser`**:

   - يتحقق مما إذا كان المستخدم من نوع **Admin** أو **SuperAdmin** أو إذا كان يحاول الوصول إلى بياناته الخاصة.
   - **الاستخدام**: يسمح للمسؤولين بالوصول إلى بيانات أي مستخدم، وللمستخدمين بالوصول إلى بياناتهم فقط.

   ```graphql
   query {
     user(id: "1") {
       name
       email
     }
   }
   ```
7. **`ServerCanRead`**:

   - وظيفة خاصة تسمح للخادم بالوصول إلى البيانات بدون التحقق من الجلسة. يتم التحقق باستخدام مفتاح الخادم المخزن في متغيرات البيئة.
   - **الاستخدام**: يستخدم في السيناريوهات التي يتعين فيها على الخادم الوصول إلى البيانات بشكل مباشر دون تدخل المستخدم.

---

### **كيف تعمل الأذونات مع GraphQL؟**

عند استخدام **Keystone** مع GraphQL، يتم استخدام هذه الوظائف للتحقق من الأذونات قبل تنفيذ أي استعلام أو عملية.

على سبيل المثال، عند محاولة المستخدم الوصول إلى بياناته الخاصة أو تحديثها، يتم استدعاء الوظيفة **`isAdminSuperAdminOrCurrentUser`** للتحقق مما إذا كان المستخدم يملك الصلاحية المناسبة. إذا كانت الشروط مستوفاة، يتم السماح بالوصول، وإذا لم تكن كذلك، يتم منع الوصول.

---

### **عرض صلاحيات المستخدم في ملفه الشخصي**

يمكن للمستخدمين عرض صلاحياتهم الحالية في ملفهم الشخصي باستخدام استعلام GraphQL يقوم بجلب دورهم وصلاحياتهم. إليك مثال لاستعلام يعرض دور المستخدم وصلاحياته:

```graphql
query getUserPermissions {
  currentUser {
    name
    role
    permissions
  }
}
```

النتيجة قد تكون مثل هذه:

```json
{
  "data": {
    "currentUser": {
      "name": "John Doe",
      "role": "admin",
      "permissions": [
        "canCreateArticles",
        "canUpdateArticles",
        "canDeleteUsers"
      ]
    }
  }
}
```

---

### **كيفية التحقق من صلاحيات المستخدم في التطبيقات**

للتأكد من أن المستخدم يمتلك الصلاحيات المناسبة للوصول إلى بعض العمليات، يتم استخدام الوظائف الخاصة بالصلاحيات المذكورة أعلاه. عند إجراء استعلام أو تعديل، يتم فحص ما إذا كان المستخدم يملك الصلاحيات المطلوبة قبل السماح بتنفيذ العملية.

### **مثال شامل:**

إذا حاول المستخدم تعديل بياناته الشخصية، فسيتم استخدام **`isAdminSuperAdminOrCurrentUser`** للتأكد من أن المستخدم هو نفس الشخص الذي يحاول تعديل بياناته، أو أنه يمتلك صلاحيات المسؤول:

```graphql
mutation updateUserProfile($id: ID!, $data: UserUpdateInput!) {
  updateUser(id: $id, data: $data) {
    id
    name
    email
  }
}
```

وفي هذه الحالة، إذا كان المستخدم لا يمتلك الصلاحيات، ستظهر رسالة خطأ تمنعه من الوصول.

---

### **معلومات اضافية عن الاذونات:**

- **الأذونات** تتحكم في من يمكنه الوصول إلى البيانات المختلفة في التطبيق.
- **Keystone** يستخدم وظائف مخصصة مثل **`isLoggedIn`** و **`isAdminOrSuperAdmin`** للتحقق من صلاحيات المستخدم.
- **GraphQL** يمكنه التفاعل مع هذه الوظائف لتقييد الوصول إلى الموارد بناءً على دور المستخدم وحالته.
- **المستخدم** يمكنه عرض صلاحياته الحالية من خلال استعلام GraphQL يعرض دوره وصلاحياته.

إذا كنت بحاجة إلى توضيحات إضافية أو تفاصيل أكثر، لا تتردد في طرحها!

### **التحقق مما إذا كان المستخدم محظورًا**

---

#### **الكود** يوضح كيفية التحقق مما إذا كان المستخدم محظورًا من خلال جلب حالة الحظر من قاعدة البيانات، بناءً على معرف المستخدم (session.itemId) الموجود في الجلسة. إذا كان المستخدم محظورًا، يتم إرجاع **`true`**.

### **كيفية الاستعلام عن حالة الحظر:**

```typescript
// هذا الكود من backend فقط يوضح كيف يعمل نظام الحظر 
export const isUserBlocked = async ({ session, context }: { session?: any; context?: any }) => {
  if (!session || !session.itemId) return false; // التحقق مما إذا كان المستخدم مسجلاً. إذا لم يكن، يتم إرجاع false.
  
  // استعلام لجلب بيانات المستخدم باستخدام معرف الجلسة
  const user = await context.query.User.findOne({
    where: { id: session.itemId }, // معرف المستخدم المستخرج من الجلسة
    query: 'isBlocked', // جلب حقل isBlocked فقط
  });
  
  return user?.isBlocked || false; // إذا كان المستخدم محظورًا، يتم إرجاع true. خلاف ذلك، يتم إرجاع false.
};
```

#### **الآلية:**

- إذا كانت الجلسة غير موجودة أو لم تحتوي على `itemId` (معرف المستخدم)، يتم إرجاع **`false`**.
- يتم استخدام **GraphQL** للاستعلام عن المستخدم بناءً على معرفه من الجلسة **(session.itemId)**.
- يتم فحص حقل **`isBlocked`** في قاعدة البيانات، وإذا كانت قيمة **`isBlocked`** تساوي **`true`**، يتم إرجاع **`true`**.
- إذا لم يكن المستخدم محظورًا، يتم إرجاع **`false`**.

### **كيفية استخدامه في فلاتر:**

#### **مثال على تطبيق Flutter للتحقق مما إذا كان المستخدم محظورًا:**

في تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع قاعدة البيانات للتحقق من حالة الحظر للمستخدم:

#### 1. إعداد GraphQL client:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: UserStatusScreen(),
      ),
    );
  }
}
```

#### 2. الاستعلام عن حالة حظر المستخدم:

يمكنك إنشاء استعلام GraphQL للتحقق مما إذا كان المستخدم محظورًا:

```graphql
query isUserBlocked($userId: ID!) {
  user(where: { id: $userId }) {
    isBlocked
  }
}
```

#### 3. تنفيذ الاستعلام في Flutter:

```dart
class UserStatusScreen extends StatelessWidget {
  final String userId = "123"; // معرف المستخدم

  final String checkUserBlockedQuery = """
    query isUserBlocked(\$userId: ID!) {
      user(where: { id: \$userId }) {
        isBlocked
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Check User Status')),
      body: Query(
        options: QueryOptions(
          document: gql(checkUserBlockedQuery),
          variables: {
            'userId': userId,
          },
        ),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text('Error: ${result.exception.toString()}');
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          // التحقق من حالة الحظر
          bool isBlocked = result.data!['user']['isBlocked'];

          return Center(
            child: isBlocked
                ? Text("User is blocked")
                : Text("User is not blocked"),
          );
        },
      ),
    );
  }
}
```

### **شرح الكود:**

1. **إعداد GraphQL Client**: يتم إعداد الاتصال بالخادم الذي يحتوي على GraphQL endpoint.
2. **إنشاء استعلام**: يتم استخدام الاستعلام **`isUserBlocked`** للتحقق مما إذا كان المستخدم محظورًا.
3. **تنفيذ الاستعلام في Flutter**: يتم تنفيذ الاستعلام باستخدام مكون **`Query`** من مكتبة **graphql_flutter**، ويتم عرض حالة المستخدم بناءً على ما إذا كان محظورًا أم لا.

---

### **الاستخدام العملي:**

- **تطبيق فعلي**: إذا كان المستخدم محظورًا، يمكن منعه من الوصول إلى بعض أو كل أجزاء التطبيق.
- **التخصيص**: بناءً على حالة الحظر، يمكن تغيير تجربة المستخدم مثل عرض رسائل تحذيرية أو حظر الوصول إلى وظائف معينة.

---

.

## GraphQL Queries and Mutations

Below are examples of how to use the API to perform CRUD operations on the `User` model.

### 1. Creating a New User (Sign Up)

To create a new user, use the following GraphQL mutation:

```graphql
mutation createUser($name: String!, $email: String!, $password: String!, $role: String!) {
  createUser(data: {
    name: $name,
    email: $email,
    password: $password,
    role: $role
  }) {
    id
    name
    email
  }
}
```

### 2. Fetching Users (Get List of Users)

To get the list of all users, use this GraphQL query:

```graphql
query getUsers {
  users {
    id
    name
    email
    role
    isBlocked
  }
}
```

### 3. Updating User Data

To update a user's data (e.g., name, email, block status), use this mutation:

```graphql
mutation updateUser($id: ID!, $name: String, $email: String, $isBlocked: Boolean) {
  updateUser(where: { id: $id }, data: { 
    name: $name, 
    email: $email, 
    isBlocked: $isBlocked 
  }) {
    id
    name
    email
    isBlocked
  }
}
```

### 4. Deleting a User

To delete a user, use the following mutation:

```graphql
mutation deleteUser($id: ID!) {
  deleteUser(where: { id: $id }) {
    id
    name
    email
  }
}
```

## Integrating with Flutter

To interact with your backend API from a Flutter application, you can use the **graphql_flutter** package. Below is an example of how to set up a simple query to fetch the list of users.

### Flutter Code Example

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: UserList(),
      ),
    );
  }
}

class UserList extends StatelessWidget {
  final String getUsersQuery = """
    query getUsers {
      users {
        id
        name
        email
        role
        isBlocked
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Users')),
      body: Query(
        options: QueryOptions(document: gql(getUsersQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List users = result.data!['users'];

          return ListView.builder(
            itemCount: users.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(users[index]['name']),
                subtitle: Text(users[index]['email']),
              );
            },
          );
        },
      ),
    );
  }
}
```

### Notes

1. Replace `https://makesbooks.com/api/graphql` with the actual URL of your API.
2. Make sure you have installed the `graphql_flutter` package in your Flutter project.

بناءً على نموذج **المستخدم (User)** المقدم، إليك قائمة بالمعلومات التي يمكن الحصول عليها أو إرسالها إلى المستخدم لكل حقل:

| الحقل            | الوصف                                                                                                                                                    |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                | المعرف الفريد للمستخدم (يتم إنشاؤه تلقائيًا بواسطة النظام).                                                  |
| `name`              | اسم المستخدم.                                                                                                                                      |
| `address`           | عنوان المستخدم (يمكن الوصول إليه من قبل المستخدم أو المشرفين فقط).                                       |
| `email`             | البريد الإلكتروني للمستخدم (يجب أن يكون فريدًا لكل مستخدم).                                                   |
| `phoneNumber`       | رقم هاتف المستخدم (خاص بالمستخدم وإدارته فقط).                                                                           |
| `oneSignalPlayerId` | معرف OneSignal الخاص بالمستخدم لتلقي الإشعارات (عند استخدامه لتلقي إشعارات عبر التطبيقات). |
| `platform`          | منصة المستخدم (iOS, Android, Web) المستخدمة للوصول إلى الخدمة.                                                            |
| `gender`            | جنس المستخدم (ذكر أو أنثى).                                                                                                               |
| `profilePicture`    | الصورة الشخصية للمستخدم (الملف الشخصي).                                                                                       |
| `coverPhoto`        | صورة الغلاف الخاصة بالمستخدم (في الصفحة الشخصية).                                                                     |
| `ijazat`            | معلومات حول "الإجازات" التي حصل عليها المستخدم (مكتوبة كـ نص).                                                |
| `completedSeries`   | عدد السلاسل المكتملة من قبل المستخدم.                                                                                          |
| `booksRead`         | عدد الكتب التي قرأها المستخدم.                                                                                                       |
| `paymentData`       | بيانات الدفع الخاصة بالمستخدم (تحتوي على معلومات الدفع مثل بطاقات الائتمان).                   |
| `isPro`             | حالة اشتراك المستخدم في الخدمة المتميزة (مشترك بريميوم).                                                        |
| `role`              | دور المستخدم (مثل: SuperAdmin, Admin, Student, etc.) يحدد صلاحياته في النظام.                                               |
| `password`          | كلمة المرور الخاصة بالمستخدم.                                                                                                        |
| `createdAt`         | تاريخ إنشاء حساب المستخدم.                                                                                                              |
| `isLive`            | حالة البث المباشر للمستخدم (يحدد ما إذا كان المستخدم يبث حاليًا).                                         |
| `isBlocked`         | حالة الحظر للمستخدم (يحدد ما إذا كان المستخدم محظورًا من الوصول أو التفاعل).                     |
| `notifications`     | قائمة بالإشعارات المرتبطة بالمستخدم.                                                                                          |
| `chats`             | قائمة بالدردشات (الذكاء الصناعي) المرتبطة بالمستخدم.                                                              |
| `telegramChannels`  | القنوات المضافة بواسطة المستخدم في Telegram.                                                                                    |
| `studentInfo`       | معلومات إضافية متعلقة بالمستخدم كـ "طالب" (مثل المستوى الحالي والكتب المفضلة).                 |

### ملاحظات:

- يمكن تعديل الحقول بناءً على صلاحيات المستخدم مثل الحقول التي يمكن أن يعدلها المشرفون فقط.
- بعض الحقول متاحة للقراءة فقط من قبل المستخدم نفسه أو المسؤولين مثل الحقول المتعلقة بالدفع أو الحالة المتميزة.

---

### **قسم معلومات الطالب (Student Info)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل         | الوصف                                                                                                   |
| ------------------ | ------------------------------------------------------------------------------------------------------------ |
| `student`        | ربط الطالب (علاقة مع قائمة المستخدمين).                                       |
| `currentSeries`  | قائمة السلاسل الحالية التي يدرسها الطالب (حقل JSON).                   |
| `currentLevel`   | المستوى الحالي للطالب (اختيار من مستويات محددة مسبقًا).         |
| `favoriteBooks`  | قائمة الكتب المفضلة للطالب (علاقة مع المكتبة).                          |
| `favoriteAudios` | قائمة الصوتيات المفضلة للطالب (علاقة مع المحتويات الصوتية). |
| `favoriteQuotes` | اقتباسات الطالب المفضلة (حقل JSON).                                                  |
| `twitterApiKey`  | مفتاح API لحساب تويتر الخاص بالطالب (نص).                                       |
| `telegramApiKey` | مفتاح API لحساب تليغرام الخاص بالطالب (نص).                                   |
| `youtubeApiKey`  | مفتاح API لحساب يوتيوب الخاص بالطالب (نص).                                     |

---

### استدعاءات الـ GraphQL لإدارة معلومات الطالب

#### 1. إضافة معلومات طالب جديدة:

```graphql
mutation createStudentInfo(
  $studentId: ID!,
  $currentSeries: JSON!,
  $currentLevel: String!,
  $favoriteBooksIds: [ID!],
  $favoriteAudiosIds: [ID!],
  $favoriteQuotes: JSON!,
  $twitterApiKey: String,
  $telegramApiKey: String,
  $youtubeApiKey: String
) {
  createStudentInfo(data: {
    student: { connect: { id: $studentId }},
    currentSeries: $currentSeries,
    currentLevel: $currentLevel,
    favoriteBooks: { connect: $favoriteBooksIds },
    favoriteAudios: { connect: $favoriteAudiosIds },
    favoriteQuotes: $favoriteQuotes,
    twitterApiKey: $twitterApiKey,
    telegramApiKey: $telegramApiKey,
    youtubeApiKey: $youtubeApiKey
  }) {
    id
    student {
      name
    }
    currentLevel
    favoriteBooks {
      title
    }
  }
}
```

#### 2. استعلام جلب معلومات الطلاب:

```graphql
query getStudentInfos {
  studentInfos {
    id
    student {
      name
    }
    currentLevel
    favoriteBooks {
      title
    }
    favoriteAudios {
      title
    }
    favoriteQuotes
  }
}
```

#### 3. تحديث معلومات الطالب:

```graphql
mutation updateStudentInfo(
  $id: ID!,
  $currentSeries: JSON!,
  $currentLevel: String!,
  $favoriteBooksIds: [ID!],
  $favoriteAudiosIds: [ID!],
  $favoriteQuotes: JSON!,
  $twitterApiKey: String,
  $telegramApiKey: String,
  $youtubeApiKey: String
) {
  updateStudentInfo(where: { id: $id }, data: {
    currentSeries: $currentSeries,
    currentLevel: $currentLevel,
    favoriteBooks: { connect: $favoriteBooksIds },
    favoriteAudios: { connect: $favoriteAudiosIds },
    favoriteQuotes: $favoriteQuotes,
    twitterApiKey: $twitterApiKey,
    telegramApiKey: $telegramApiKey,
    youtubeApiKey: $youtubeApiKey
  }) {
    id
    student {
      name
    }
  }
}
```

#### 4. حذف معلومات الطالب:

```graphql
mutation deleteStudentInfo($id: ID!) {
  deleteStudentInfo(where: { id: $id }) {
    id
    student {
      name
    }
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع معلومات الطلاب. إليك مثال لجلب معلومات الطلاب:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: StudentInfoList(),
      ),
    );
  }
}

class StudentInfoList extends StatelessWidget {
  final String getStudentInfosQuery = """
    query getStudentInfos {
      studentInfos {
        id
        student {
          name
        }
        currentLevel
        favoriteBooks {
          title
        }
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Student Info')),
      body: Query(
        options: QueryOptions(document: gql(getStudentInfosQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List studentInfos = result.data!['studentInfos'];

          return ListView.builder(
            itemCount: studentInfos.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(studentInfos[index]['student']['name']),
                subtitle: Text('Level: ${studentInfos[index]['currentLevel']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **السلاسل الحالية**: يتم تخزين السلاسل التي يتابعها الطالب في حقل JSON.
- **المستوى**: يحدد المستوى الدراسي الذي يتبعه الطالب.
- **الكتب والصوتيات المفضلة**: يمكن ربط الكتب والصوتيات المفضلة التي يفضلها الطالب.
- **مفاتيح API**: حقول لتخزين مفاتيح API المتعلقة بتويتر، تليغرام، ويوتيوب.

---

.

### **قسم سجل الإشعارات (Notification History)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل             | الوصف                                                                                          |
| ---------------------- | --------------------------------------------------------------------------------------------------- |
| `title`              | عنوان الإشعار (نص مطلوب).                                                        |
| `message`            | نص الإشعار (نص مطلوب).                                                              |
| `notificationNumber` | رقم الإشعار، يتم استخدامه لترتيب الإشعارات (مطلوب).       |
| `user`               | المستخدم المرتبط بالإشعار (علاقة مع قائمة المستخدمين). |

---

### استدعاءات الـ GraphQL لإدارة الإشعارات

#### 1. إضافة إشعار جديد:

```graphql
mutation createNotification($title: String!, $message: String!, $notificationNumber: Int!, $userId: ID!) {
  createNotificationHistory(data: {
    title: $title,
    message: $message,
    notificationNumber: $notificationNumber,
    user: { connect: { id: $userId }}
  }) {
    id
    title
    message
    notificationNumber
  }
}
```

#### 2. استعلام جلب الإشعارات:

```graphql
query getNotifications {
  notificationHistories {
    id
    title
    message
    notificationNumber
    user {
      name
    }
  }
}
```

#### 3. تحديث إشعار:

```graphql
mutation updateNotification($id: ID!, $title: String!, $message: String!, $notificationNumber: Int!) {
  updateNotificationHistory(where: { id: $id }, data: {
    title: $title,
    message: $message,
    notificationNumber: $notificationNumber
  }) {
    id
    title
    message
  }
}
```

#### 4. حذف إشعار:

```graphql
mutation deleteNotification($id: ID!) {
  deleteNotificationHistory(where: { id: $id }) {
    id
    title
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع الإشعارات، مثل عرض الإشعارات الخاصة بالمستخدم أو إضافة إشعارات جديدة. إليك مثال لجلب الإشعارات:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: NotificationList(),
      ),
    );
  }
}

class NotificationList extends StatelessWidget {
  final String getNotificationsQuery = """
    query getNotifications {
      notificationHistories {
        id
        title
        message
        notificationNumber
        user {
          name
        }
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Notifications')),
      body: Query(
        options: QueryOptions(document: gql(getNotificationsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List notifications = result.data!['notificationHistories'];

          return ListView.builder(
            itemCount: notifications.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(notifications[index]['title']),
                subtitle: Text('User: ${notifications[index]['user']['name']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **فرز الإشعارات**: يتم فرز الإشعارات بشكل تلقائي حسب رقم الإشعار بترتيب تنازلي لعرض أحدث الإشعارات أولاً.
- **ربط المستخدم**: كل إشعار مرتبط بمستخدم معين، ويمكن جلب الإشعارات حسب المستخدم إذا لزم الأمر.

---

.

# قسم الناشرين (Publishers)

#### قائمة الناشرين

| الحقل             | الوصف                                                                                                                          |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `name`               | اسم الناشر (نص مطلوب).                                                                                              |
| `account`            | ارتباط بحساب مستخدم واحد فقط يمثل الناشر.                                                         |
| `role`               | دور المستخدم، وهو "Publisher" بشكل افتراضي.                                                               |
| `distributeArticles` | تحديد ما إذا كان الناشر يقوم بتوزيع المقالات على قنوات التلغرام (نعم/لا). |

---

### **دور قائمة الناشرين**

القائمة تمثل **الناشرين**، وهي جزء أساسي يتيح ربط الأدوات والمحتويات ب**حساب مستخدم محدد** يعمل كناشر. من خلال قائمة الناشرين، يمكن تخصيص الأدوات والمحتويات بشكل مستقل لكل ناشر. فمثلاً:

- عند تحديد **الناشر "محمد بن شمس الدين"**، يتم ربط جميع الاستعلامات والمقالات والدروس والصوتيات بحساب محمد فقط.
- يمكن عرض الواجهة أو الأدوات أو المحتويات بناءً على الناشر الذي تم اختياره.
- إذا لم يتم اختيار ناشر محدد، يتم عرض جميع المقالات والمنشورات من جميع الناشرين.

### **التطبيق العملي في Flutter**

#### 1. عرض قائمة الناشرين بطريقة جذابة:

عند تشغيل التطبيق، يتم عرض **قائمة الناشرين في صور دائرية في وسط الصفحة** مع تصميم عشوائي لجذب الانتباه.

#### 2. تخزين الناشر المختار:

عند اختيار ناشر، يتم **تخزينه في متغير محلي**، ويتم استخدام هذا المتغير في جميع الاستعلامات المتعلقة بالمقالات والدروس والصوتيات.

- إذا تم اختيار ناشر معين، يتم عرض البيانات الخاصة به فقط.
- إذا ترك المتغير فارغًا، يتم الاستعلام عن جميع المقالات والمنشورات من جميع الناشرين.

---

### **التكامل مع Flutter**

إليك مثال لعرض قائمة الناشرين في **صور دائرية عشوائية**:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';
import 'dart:math';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: PublisherList(),
      ),
    );
  }
}

class PublisherList extends StatefulWidget {
  @override
  _PublisherListState createState() => _PublisherListState();
}

class _PublisherListState extends State<PublisherList> {
  String? selectedPublisherId;

  final String getPublishersQuery = """
    query getPublishers {
      publishers {
        id
        name
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Publishers')),
      body: Query(
        options: QueryOptions(document: gql(getPublishersQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List publishers = result.data!['publishers'];

          return GridView.builder(
            gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
              crossAxisCount: 2,
              mainAxisSpacing: 10,
              crossAxisSpacing: 10,
            ),
            itemCount: publishers.length,
            itemBuilder: (context, index) {
              // ترتيب عشوائي لجعل العرض أكثر جذبًا
              final randomIndex = Random().nextInt(publishers.length);
              final publisher = publishers[randomIndex];

              return GestureDetector(
                onTap: () {
                  setState(() {
                    selectedPublisherId = publisher['id'];
                  });
                },
                child: Column(
                  children: [
                    CircleAvatar(
                      radius: 40,
                      child: Text(publisher['name'][0]), // عرض أول حرف من اسم الناشر
                    ),
                    SizedBox(height: 10),
                    Text(publisher['name']),
                  ],
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          if (selectedPublisherId != null) {
            // يمكن استدعاء استعلامات تخص هذا الناشر فقط بناءً على `selectedPublisherId`
            print('تم اختيار الناشر: $selectedPublisherId');
          } else {
            print('عرض جميع البيانات لجميع الناشرين');
          }
        },
        child: Icon(Icons.check),
      ),
    );
  }
}
```

---

### **الاستخدام الفعلي:**

- **اختيار ناشر**: عند النقر على صورة دائرية للناشر، يتم حفظ **معرّف الناشر** في متغير محلي.
- **تصفية الاستعلامات**: بناءً على الناشر المختار، يمكن تصفية الاستعلامات المتعلقة بالمقالات أو الدروس أو الصوتيات. إذا لم يتم اختيار ناشر، سيتم جلب جميع البيانات لجميع الناشرين.
- **واجهة جذابة**: العرض العشوائي للناشرين في شكل صور دائرية يضيف جمالية إلى التطبيق ويجعله أكثر جذبًا.

---

.

### **قسم المقالات (Articles)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل           | الوصف                                                                                                                          |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `title`            | عنوان المقال.                                                                                                            |
| `shortDescription` | وصف مختصر للمقال يوضح محتواه بشكل موجز.                                                             |
| `publisher`        | علاقة بالناشر الذي نشر المقال. يتم اختياره من قائمة الناشرين.                     |
| `content`          | النص الكامل للمقال. يمكن تحريره باستخدام محرر نصوص احترافي.                        |
| `coverImage`       | صورة الغلاف المرفقة بالمقال.                                                                                |
| `keywords`         | الكلمات المفتاحية المرتبطة بالمقال، والتي تساعد في تحسين السيو (SEO).         |
| `metatag`          | الميتا تاغ المستخدمة لأغراض تحسين محركات البحث (SEO).                                       |
| `category`         | الفئة التي ينتمي إليها المقال، ويتم اختيارها من قائمة الفئات المتاحة.     |
| `type`             | نوع المقال (إخباري، عقدي، للطلاب، عام) وفقًا لما يحدده الناشر.                    |
| `publishInSchool`  | تحديد ما إذا كان سيتم نشر المقال في المدرسة (Checkbox).                                          |
| `isActive`         | حالة المقال (نشط أو غير نشط) لتحديد ما إذا كان المقال مرئيًا للقراء أم لا. |

---

### استدعاءات الـ GraphQL للمقالات

#### 1. إنشاء مقال جديد:

لإضافة مقال جديد، يمكنك استخدام الاستعلام التالي:

```graphql
mutation createArticle($title: String!, $shortDescription: String!, $content: String!, $publisherId: ID!, $categoryId: ID!, $type: String!) {
  createArticle(data: {
    title: $title,
    shortDescription: $shortDescription,
    content: $content,
    publisher: { connect: { id: $publisherId }},
    category: { connect: { id: $categoryId }},
    type: $type
  }) {
    id
    title
    shortDescription
    content
  }
}
```

#### 2. استعلام جلب المقالات:

للحصول على قائمة بالمقالات:

```graphql
query getArticles {
  articles {
    id
    title
    shortDescription
    content
    category {
      name
    }
    publisher {
      name
    }
    type
    isActive
  }
}
```

#### 3. تحديث مقال:

لتحديث مقال معين، استخدم هذا الاستعلام:

```graphql
mutation updateArticle($id: ID!, $title: String, $shortDescription: String, $content: String, $isActive: String) {
  updateArticle(where: { id: $id }, data: { 
    title: $title, 
    shortDescription: $shortDescription, 
    content: $content,
    isActive: $isActive
  }) {
    id
    title
    shortDescription
    isActive
  }
}
```

#### 4. حذف مقال:

لحذف مقال معين، استخدم الاستعلام التالي:

```graphql
mutation deleteArticle($id: ID!) {
  deleteArticle(where: { id: $id }) {
    id
    title
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع Flutter، يمكنك استخدام مكتبة **graphql_flutter** لتنفيذ استعلامات GraphQL. المثال التالي يوضح كيفية الحصول على قائمة المقالات:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: ArticleList(),
      ),
    );
  }
}

class ArticleList extends StatelessWidget {
  final String getArticlesQuery = """
    query getArticles {
      articles {
        id
        title
        shortDescription
        content
        category {
          name
        }
        publisher {
          name
        }
        type
        isActive
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Articles')),
      body: Query(
        options: QueryOptions(document: gql(getArticlesQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List articles = result.data!['articles'];

          return ListView.builder(
            itemCount: articles.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(articles[index]['title']),
                subtitle: Text(articles[index]['shortDescription']),
              );
            },
          );
        },
      ),
    );
  }
}
```

### **بقية الأدوات:**

- **صورة الغلاف**: تستخدم لعرض صورة مميزة لكل مقال.
- **الكلمات المفتاحية**: تساعد في تحسين قابلية البحث عن المقالات باستخدام محركات البحث.
- **Meta Tag**: تحسين تحسين محركات البحث (SEO) عبر الميتا تاغ.
- **الفئة**: تحديد الفئة التي ينتمي إليها المقال لتسهيل تصنيفه.
- **حالة المقال**: يمكن التحكم في ما إذا كان المقال نشطًا أو غير نشط.

---

.

### **قسم المرفقات (Attachments)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل      | الوصف                                                                                                 |
| --------------- | ---------------------------------------------------------------------------------------------------------- |
| `file`        | الملف المرفق (يتم تخزينه في مسار محدد).                                      |
| `description` | وصف المرفق (نص).                                                                                |
| `lesson`      | علاقة بالدرس المرتبط بهذا المرفق.                                              |
| `uploadedAt`  | تاريخ ووقت رفع المرفق (يتم تعيينه تلقائيًا عند رفع المرفق). |

---

### استدعاءات الـ GraphQL للمرفقات

#### 1. إضافة مرفق جديد:

لإضافة مرفق جديد، استخدم الاستعلام التالي:

```graphql
mutation createAttachment($file: Upload!, $description: String, $lessonId: ID!) {
  createAttachment(data: {
    file: $file,
    description: $description,
    lesson: { connect: { id: $lessonId }}
  }) {
    id
    file {
      url
    }
    description
    lesson {
      id
      title
    }
    uploadedAt
  }
}
```

#### 2. استعلام جلب المرفقات:

للحصول على قائمة بالمرفقات:

```graphql
query getAttachments {
  attachments {
    id
    file {
      url
    }
    description
    lesson {
      title
    }
    uploadedAt
  }
}
```

#### 3. تحديث مرفق:

لتحديث مرفق معين:

```graphql
mutation updateAttachment($id: ID!, $description: String) {
  updateAttachment(where: { id: $id }, data: { 
    description: $description
  }) {
    id
    description
    uploadedAt
  }
}
```

#### 4. حذف مرفق:

لحذف مرفق معين:

```graphql
mutation deleteAttachment($id: ID!) {
  deleteAttachment(where: { id: $id }) {
    id
    description
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع الـ API. إليك مثال للحصول على قائمة المرفقات:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: AttachmentList(),
      ),
    );
  }
}

class AttachmentList extends StatelessWidget {
  final String getAttachmentsQuery = """
    query getAttachments {
      attachments {
        id
        file {
          url
        }
        description
        lesson {
          title
        }
        uploadedAt
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Attachments')),
      body: Query(
        options: QueryOptions(document: gql(getAttachmentsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List attachments = result.data!['attachments'];

          return ListView.builder(
            itemCount: attachments.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(attachments[index]['description'] ?? 'No description'),
                subtitle: Text('Lesson: ${attachments[index]['lesson']['title']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **الملف**: يمثل الملف المرفق الذي يتم رفعه من قبل المستخدم.
- **الوصف**: وصف مختصر للمرفق لتوضيح محتواه أو غرضه.
- **الدرس المرتبط**: الدرس الذي يرتبط به هذا المرفق.
- **تاريخ الرفع**: يتم تعيينه تلقائيًا عند رفع الملف لتتبع وقت الرفع.

---

.

### **قسم المحتوى الصوتي (Audio Content)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل      | الوصف                                                   |
| --------------- | ------------------------------------------------------------ |
| `title`       | عنوان المحتوى الصوتي (نص مطلوب).    |
| `fileUrl`     | رابط ملف الصوت الذي يتم استضافته. |
| `description` | وصف مختصر للمحتوى الصوتي.               |

---

### استدعاءات الـ GraphQL للمحتوى الصوتي

#### 1. إضافة محتوى صوتي جديد:

لإضافة محتوى صوتي جديد، استخدم الاستعلام التالي:

```graphql
mutation createAudioContent($title: String!, $fileUrl: String!, $description: String) {
  createAudioContent(data: {
    title: $title,
    fileUrl: $fileUrl,
    description: $description
  }) {
    id
    title
    fileUrl
    description
  }
}
```

#### 2. استعلام جلب المحتوى الصوتي:

للحصول على قائمة بالمحتوى الصوتي:

```graphql
query getAudioContents {
  audioContents {
    id
    title
    fileUrl
    description
  }
}
```

#### 3. تحديث محتوى صوتي:

لتحديث محتوى صوتي معين:

```graphql
mutation updateAudioContent($id: ID!, $title: String, $fileUrl: String, $description: String) {
  updateAudioContent(where: { id: $id }, data: { 
    title: $title,
    fileUrl: $fileUrl,
    description: $description
  }) {
    id
    title
    fileUrl
    description
  }
}
```

#### 4. حذف محتوى صوتي:

لحذف محتوى صوتي معين:

```graphql
mutation deleteAudioContent($id: ID!) {
  deleteAudioContent(where: { id: $id }) {
    id
    title
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع Flutter، استخدم مكتبة **graphql_flutter** للتفاعل مع الـ API. إليك مثال للحصول على قائمة المحتوى الصوتي:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: AudioContentList(),
      ),
    );
  }
}

class AudioContentList extends StatelessWidget {
  final String getAudioContentsQuery = """
    query getAudioContents {
      audioContents {
        id
        title
        fileUrl
        description
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Audio Contents')),
      body: Query(
        options: QueryOptions(document: gql(getAudioContentsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List audioContents = result.data!['audioContents'];

          return ListView.builder(
            itemCount: audioContents.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(audioContents[index]['title']),
                subtitle: Text(audioContents[index]['description'] ?? 'No description'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **رابط الملف الصوتي (fileUrl)**: يحتوي على الرابط المباشر لتحميل أو تشغيل ملف الصوت.
- **الوصف**: يحتوي على معلومات إضافية أو وصف لمحتوى الصوت.
- **العنوان**: يوضح اسم أو عنوان المحتوى الصوتي.

---

.

### **قسم الفئات الرئيسية والفئات الفرعية (Categories & Category Sections)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

##### الفئات الرئيسية (Category):

| الحقل | الوصف                                            |
| ---------- | ----------------------------------------------------- |
| `name`   | اسم الفئة الرئيسية (نص مطلوب). |

##### الفئات الفرعية (Category Section):

| الحقل         | الوصف                                                                               |
| ------------------ | ---------------------------------------------------------------------------------------- |
| `name`           | اسم الفئة الفرعية (نص مطلوب).                                      |
| `parentCategory` | الفئة الرئيسية التي ترتبط بها هذه الفئة الفرعية. |
| `subCategory`    | الفئة الفرعية المرتبطة إذا كانت موجودة.                 |

---

### استدعاءات الـ GraphQL للفئات الرئيسية والفئات الفرعية

#### 1. إضافة فئة رئيسية جديدة:

لإضافة فئة رئيسية جديدة، استخدم الاستعلام التالي:

```graphql
mutation createCategory($name: String!) {
  createCategory(data: {
    name: $name
  }) {
    id
    name
  }
}
```

#### 2. استعلام جلب الفئات الرئيسية:

للحصول على قائمة بالفئات الرئيسية:

```graphql
query getCategories {
  categories {
    id
    name
  }
}
```

#### 3. إضافة فئة فرعية جديدة:

لإضافة فئة فرعية جديدة، استخدم الاستعلام التالي:

```graphql
mutation createCategorySection($name: String!, $parentCategoryId: ID, $subCategoryId: ID) {
  createCategorySection(data: {
    name: $name,
    parentCategory: { connect: { id: $parentCategoryId }},
    subCategory: { connect: { id: $subCategoryId }}
  }) {
    id
    name
    parentCategory {
      id
      name
    }
    subCategory {
      id
      name
    }
  }
}
```

#### 4. استعلام جلب الفئات الفرعية:

للحصول على قائمة بالفئات الفرعية:

```graphql
query getCategorySections {
  categorySections {
    id
    name
    parentCategory {
      name
    }
    subCategory {
      name
    }
  }
}
```

#### 5. تحديث فئة رئيسية أو فرعية:

لتحديث فئة رئيسية أو فرعية:

```graphql
mutation updateCategory($id: ID!, $name: String!) {
  updateCategory(where: { id: $id }, data: { name: $name }) {
    id
    name
  }
}

mutation updateCategorySection($id: ID!, $name: String!, $parentCategoryId: ID, $subCategoryId: ID) {
  updateCategorySection(where: { id: $id }, data: { 
    name: $name,
    parentCategory: { connect: { id: $parentCategoryId }},
    subCategory: { connect: { id: $subCategoryId }}
  }) {
    id
    name
  }
}
```

#### 6. حذف فئة رئيسية أو فرعية:

لحذف فئة رئيسية أو فرعية:

```graphql
mutation deleteCategory($id: ID!) {
  deleteCategory(where: { id: $id }) {
    id
    name
  }
}

mutation deleteCategorySection($id: ID!) {
  deleteCategorySection(where: { id: $id }) {
    id
    name
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع Flutter، يمكنك استخدام مكتبة **graphql_flutter** للحصول على قائمة الفئات الرئيسية والفئات الفرعية، أو لإجراء تعديلات عليها. إليك مثال للحصول على قائمة الفئات الرئيسية:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: CategoryList(),
      ),
    );
  }
}

class CategoryList extends StatelessWidget {
  final String getCategoriesQuery = """
    query getCategories {
      categories {
        id
        name
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Categories')),
      body: Query(
        options: QueryOptions(document: gql(getCategoriesQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List categories = result.data!['categories'];

          return ListView.builder(
            itemCount: categories.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(categories[index]['name']),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **الفئة الرئيسية (Category)**: الفئة الرئيسية التي يتم إدخالها في النظام لتصنيف المحتويات.
- **الفئة الفرعية (Category Section)**: فئة فرعية يمكن أن ترتبط بفئة رئيسية أو فئة فرعية أخرى لتوسيع الهيكل التنظيمي.

---

.

### **قسم طلبات الإجازات والإجازات (Certification Requests & Certifications)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

##### طلبات الإجازات (Certification Request):

| الحقل           | الوصف                                                                                     |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| `certificateCode`  | رمز توثيق الشهادة (يجب أن يكون فريدًا).                          |
| `recipientName`    | اسم الحاصل على الإجازة.                                                     |
| `publisher`        | الناشر المسؤول عن الإجازة (علاقة مع قائمة الناشرين). |
| `bookTitle`        | اسم الكتاب الذي حصلت الإجازة عليه.                                 |
| `method`           | نوع الإجازة (سماعي، على الواقع، قراءة).                         |
| `explanationTitle` | اسم الشرح المرفق مع الإجازة.                                            |
| `recipientEmail`   | البريد الإلكتروني للحاصل على الإجازة.                          |
| `issuedAt`         | تاريخ إصدار الإجازة.                                                          |
| `status`           | حالة الطلب (قيد الانتظار، مقبول، مرفوض).                       |

##### الإجازات (Certification):

| الحقل           | الوصف                                                                                                                                                |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `certificateCode`  | رمز توثيق الشهادة (يجب أن يكون فريدًا).                                                                                     |
| `recipientName`    | اسم الحاصل على الإجازة.                                                                                                                |
| `publisher`        | الناشر المسؤول عن الإجازة (علاقة مع قائمة الناشرين).                                                            |
| `bookTitle`        | اسم الكتاب الذي حصلت الإجازة عليه.                                                                                            |
| `method`           | نوع الإجازة (سماعي، على الواقع، قراءة).                                                                                    |
| `explanationTitle` | اسم الشرح المرفق مع الإجازة.                                                                                                       |
| `recipient`        | اسم الحاصل على الإجازة (علاقة مع قائمة المستخدمين بالبحث عن طريق البريد الإلكتروني). |
| `issuedAt`         | تاريخ إصدار الإجازة.                                                                                                                     |

---

### استدعاءات الـ GraphQL لطلبات الإجازات والإجازات

#### 1. إضافة طلب إجازة جديد:

لإضافة طلب إجازة جديد، استخدم الاستعلام التالي:

```graphql
mutation createCertificationRequest($certificateCode: String!, $recipientName: String!, $bookTitle: String!, $method: String!, $explanationTitle: String!, $recipientEmail: String!) {
  createCertificationRequest(data: {
    certificateCode: $certificateCode,
    recipientName: $recipientName,
    bookTitle: $bookTitle,
    method: $method,
    explanationTitle: $explanationTitle,
    recipientEmail: $recipientEmail
  }) {
    id
    certificateCode
    recipientName
    bookTitle
    status
  }
}
```

#### 2. استعلام جلب طلبات الإجازات:

للحصول على قائمة بطلبات الإجازات:

```graphql
query getCertificationRequests {
  certificationRequests {
    id
    certificateCode
    recipientName
    bookTitle
    status
  }
}
```

#### 3. إضافة إجازة جديدة:

لإضافة إجازة جديدة، استخدم الاستعلام التالي:

```graphql
mutation createCertification($certificateCode: String!, $recipientName: String!, $bookTitle: String!, $method: String!, $publisherId: ID!) {
  createCertification(data: {
    certificateCode: $certificateCode,
    recipientName: $recipientName,
    bookTitle: $bookTitle,
    method: $method,
    publisher: { connect: { id: $publisherId }}
  }) {
    id
    certificateCode
    recipientName
    bookTitle
  }
}
```

#### 4. استعلام جلب الإجازات:

للحصول على قائمة بالإجازات:

```graphql
query getCertifications {
  certifications {
    id
    certificateCode
    recipientName
    bookTitle
    publisher {
      name
    }
    method
  }
}
```

#### 5. تحديث طلب إجازة أو إجازة:

لتحديث طلب إجازة أو إجازة:

```graphql
mutation updateCertificationRequest($id: ID!, $status: String!) {
  updateCertificationRequest(where: { id: $id }, data: { status: $status }) {
    id
    status
  }
}

mutation updateCertification($id: ID!, $certificateCode: String!, $recipientName: String!, $bookTitle: String!) {
  updateCertification(where: { id: $id }, data: { 
    certificateCode: $certificateCode,
    recipientName: $recipientName,
    bookTitle: $bookTitle
  }) {
    id
    certificateCode
    recipientName
  }
}
```

#### 6. حذف طلب إجازة أو إجازة:

لحذف طلب إجازة أو إجازة:

```graphql
mutation deleteCertificationRequest($id: ID!) {
  deleteCertificationRequest(where: { id: $id }) {
    id
    certificateCode
  }
}

mutation deleteCertification($id: ID!) {
  deleteCertification(where: { id: $id }) {
    id
    certificateCode
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع طلبات الإجازات والإجازات. إليك مثال للحصول على قائمة طلبات الإجازات:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: CertificationRequestList(),
      ),
    );
  }
}

class CertificationRequestList extends StatelessWidget {
  final String getCertificationRequestsQuery = """
    query getCertificationRequests {
      certificationRequests {
        id
        certificateCode
        recipientName
        bookTitle
        status
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Certification Requests')),
      body: Query(
        options: QueryOptions(document: gql(getCertificationRequestsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List certificationRequests = result.data!['certificationRequests'];

          return ListView.builder(
            itemCount: certificationRequests.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(certificationRequests[index]['recipientName']),
                subtitle: Text('Book: ${certificationRequests[index]['bookTitle']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **رمز توثيق الشهادة (certificateCode)**: الرمز الفريد لكل إجازة أو طلب إجازة.
- **الحالة (status)**: الحالة الحالية لطلب الإجازة (قيد الانتظار، مقبول، مرفوض).
- **الإشعارات**: يتم إرسال إشعارات إلى المستخدم عند قبول طلب الإجازة باستخدام خدمة **OneSignal**.

---

.

### **قسم المسابقات وأسئلة المسابقات (Competitions & Competition Questions)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

##### المسابقات (Competition):

| الحقل          | الوصف                                                               |
| ------------------- | ------------------------------------------------------------------------ |
| `name`            | اسم المسابقة (نص مطلوب).                               |
| `book`            | كتاب مرتبط بالمسابقة (علاقة مع المكتبة). |
| `type`            | نوع المسابقة (نشط، غير نشط، عرض ترويجي).  |
| `prize`           | الجائزة المرفقة للمسابقة (نص اختياري).    |
| `startDate`       | تاريخ بدء المسابقة (مطلوب).                         |
| `competitionTime` | مدة المسابقة (بالدقائق).                              |
| `questionTime`    | مدة السؤال (بالثواني).                                  |

##### أسئلة المسابقة (Competition Question):

| الحقل        | الوصف                                                                 |
| ----------------- | -------------------------------------------------------------------------- |
| `questionText`  | نص السؤال (مطلوب).                                            |
| `competition`   | المسابقة المرتبطة بالسؤال (علاقة).             |
| `correctAnswer` | الإجابة الصحيحة للسؤال.                                |
| `answers`       | الأجوبة المتاحة للسؤال (نص متعدد الأسطر). |
| `answerCount`   | عدد الأجوبة المتاحة للسؤال (2-6 أجوبة).        |

---

### استدعاءات الـ GraphQL للمسابقات وأسئلة المسابقات

#### 1. إضافة مسابقة جديدة:

```graphql
mutation createCompetition($name: String!, $bookId: ID!, $type: String!, $prize: String, $startDate: DateTime!, $competitionTime: Int!, $questionTime: Int!) {
  createCompetition(data: {
    name: $name,
    book: { connect: { id: $bookId }},
    type: $type,
    prize: $prize,
    startDate: $startDate,
    competitionTime: $competitionTime,
    questionTime: $questionTime
  }) {
    id
    name
    type
    startDate
  }
}
```

#### 2. استعلام جلب المسابقات:

```graphql
query getCompetitions {
  competitions {
    id
    name
    type
    startDate
    competitionTime
    prize
  }
}
```

#### 3. إضافة سؤال مسابقة:

```graphql
mutation createCompetitionQuestion($questionText: String!, $competitionId: ID!, $correctAnswer: String!, $answers: String!, $answerCount: Int!) {
  createCompetitionQuestion(data: {
    questionText: $questionText,
    competition: { connect: { id: $competitionId }},
    correctAnswer: $correctAnswer,
    answers: $answers,
    answerCount: $answerCount
  }) {
    id
    questionText
    correctAnswer
  }
}
```

#### 4. استعلام جلب أسئلة المسابقة:

```graphql
query getCompetitionQuestions {
  competitionQuestions {
    id
    questionText
    competition {
      name
    }
    correctAnswer
    answers
  }
}
```

#### 5. تحديث مسابقة أو سؤال:

```graphql
mutation updateCompetition($id: ID!, $name: String!, $type: String!, $startDate: DateTime!, $competitionTime: Int!) {
  updateCompetition(where: { id: $id }, data: {
    name: $name,
    type: $type,
    startDate: $startDate,
    competitionTime: $competitionTime
  }) {
    id
    name
    type
  }
}

mutation updateCompetitionQuestion($id: ID!, $questionText: String!, $correctAnswer: String!, $answers: String!) {
  updateCompetitionQuestion(where: { id: $id }, data: {
    questionText: $questionText,
    correctAnswer: $correctAnswer,
    answers: $answers
  }) {
    id
    questionText
    correctAnswer
  }
}
```

#### 6. حذف مسابقة أو سؤال:

```graphql
mutation deleteCompetition($id: ID!) {
  deleteCompetition(where: { id: $id }) {
    id
    name
  }
}

mutation deleteCompetitionQuestion($id: ID!) {
  deleteCompetitionQuestion(where: { id: $id }) {
    id
    questionText
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للحصول على قائمة المسابقات أو أسئلة المسابقات. إليك مثال لجلب قائمة المسابقات:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: CompetitionList(),
      ),
    );
  }
}

class CompetitionList extends StatelessWidget {
  final String getCompetitionsQuery = """
    query getCompetitions {
      competitions {
        id
        name
        type
        startDate
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Competitions')),
      body: Query(
        options: QueryOptions(document: gql(getCompetitionsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List competitions = result.data!['competitions'];

          return ListView.builder(
            itemCount: competitions.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(competitions[index]['name']),
                subtitle: Text('Type: ${competitions[index]['type']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **نوع المسابقة (type)**: نوع المسابقة يحدد حالتها (نشط، غير نشط، عرض ترويجي).
- **إرسال الإشعارات**: يتم إرسال الإشعارات عبر خدمة **OneSignal** بناءً على نوع المسابقة.
- **تحديد الفائزين**: بناءً على إجابات أسئلة المسابقة، يمكن تحديد الفائزين.

---

.

### **قسم الدروس اليومية (Daily Lessons)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل      | الوصف                                                                                           |
| --------------- | ---------------------------------------------------------------------------------------------------- |
| `title`       | اسم الدرس (نص مطلوب).                                                                 |
| `type`        | نوع المحتوى (كتابي، صوتي، مرئي).                                            |
| `content`     | النص الكامل للدرس إذا كان "كتابي".                                         |
| `audioFile`   | ملف صوتي للدرس (يستخدم عند اختيار نوع "صوتي").                     |
| `videoLink`   | رابط الفيديو للدرس (يستخدم عند اختيار نوع "مرئي").             |
| `description` | وصف مختصر للدرس (نص متعدد الأسطر).                                         |
| `series`      | السلسلة التي ينتمي إليها الدرس (علاقة مع سلسلة الدروس).  |
| `book`        | الكتاب المرتبط بالدرس (علاقة مع المكتبة).                           |
| `article`     | المقال المرتبط بالدرس (علاقة مع المقالات).                         |
| `level`       | المستوى الذي يتبع له الدرس (مستوى أول، مستوى ثاني، إلخ). |
| `order`       | رقم ترتيب الدرس داخل السلسلة.                                                |
| `date`        | تاريخ إضافة الدرس (تعيين تلقائي عند الإضافة).                    |

---

### استدعاءات الـ GraphQL للدروس اليومية

#### 1. إضافة درس يومي جديد:

```graphql
mutation createDailyLesson($title: String!, $type: String!, $content: String, $audioFile: Upload, $videoLink: String, $description: String, $seriesId: ID, $bookId: ID, $articleId: ID, $level: String!, $order: Int, $date: DateTime!) {
  createDailyLesson(data: {
    title: $title,
    type: $type,
    content: $content,
    audioFile: $audioFile,
    videoLink: $videoLink,
    description: $description,
    series: { connect: { id: $seriesId }},
    book: { connect: { id: $bookId }},
    article: { connect: { id: $articleId }},
    level: $level,
    order: $order,
    date: $date
  }) {
    id
    title
    type
    description
    date
  }
}
```

#### 2. استعلام جلب الدروس اليومية:

```graphql
query getDailyLessons {
  dailyLessons {
    id
    title
    type
    description
    series {
      title
    }
    level
    order
    date
  }
}
```

#### 3. تحديث درس يومي:

```graphql
mutation updateDailyLesson($id: ID!, $title: String, $type: String, $content: String, $audioFile: Upload, $videoLink: String, $description: String, $seriesId: ID, $bookId: ID, $articleId: ID, $level: String, $order: Int, $date: DateTime) {
  updateDailyLesson(where: { id: $id }, data: {
    title: $title,
    type: $type,
    content: $content,
    audioFile: $audioFile,
    videoLink: $videoLink,
    description: $description,
    series: { connect: { id: $seriesId }},
    book: { connect: { id: $bookId }},
    article: { connect: { id: $articleId }},
    level: $level,
    order: $order,
    date: $date
  }) {
    id
    title
    type
    description
  }
}
```

#### 4. حذف درس يومي:

```graphql
mutation deleteDailyLesson($id: ID!) {
  deleteDailyLesson(where: { id: $id }) {
    id
    title
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع الدروس اليومية. إليك مثال لجلب قائمة الدروس اليومية:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: DailyLessonList(),
      ),
    );
  }
}

class DailyLessonList extends StatelessWidget {
  final String getDailyLessonsQuery = """
    query getDailyLessons {
      dailyLessons {
        id
        title
        type
        description
        series {
          title
        }
        level
        order
        date
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Daily Lessons')),
      body: Query(
        options: QueryOptions(document: gql(getDailyLessonsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List dailyLessons = result.data!['dailyLessons'];

          return ListView.builder(
            itemCount: dailyLessons.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(dailyLessons[index]['title']),
                subtitle: Text('Level: ${dailyLessons[index]['level']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **نوع المحتوى (type)**: يمكن أن يكون نوع الدرس "كتابي"، "صوتي"، أو "مرئي".
- **ملف الصوت**: يستخدم عند اختيار نوع "صوتي" لتحميل ملف صوتي.
- **رابط الفيديو**: يستخدم عند اختيار نوع "مرئي" لإدخال رابط الفيديو.
- **المستوى**: يحدد مستوى الدرس سواء كان للمبتدئين أو المتقدمين.

---

.

### **قسم البرامج التفاعلية (Interactive Programs)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل      | الوصف                                                                                    |
| --------------- | --------------------------------------------------------------------------------------------- |
| `name`        | اسم البرنامج التفاعلي (نص مطلوب).                                   |
| `attachments` | مرفقات البرنامج (مثل ملفات PDF أو صور يتم رفعها).          |
| `youtubeLink` | رابط فيديو يوتيوب (اختياري).                                            |
| `books`       | الكتب المرتبطة بالبرنامج (علاقة مع المكتبة).              |
| `series`      | السلسلة المرتبطة بالبرنامج (علاقة مع سلسلة الدروس). |
| `description` | وصف مختصر للبرنامج (نص متعدد الأسطر).                            |
| `keywords`    | الكلمات المفتاحية التي تسهل البحث عن البرنامج.         |
| `coverImage`  | صورة غلاف للبرنامج (يتم رفعها).                                       |
| `isActive`    | حالة البرنامج (نشط أو غير نشط).                                        |

---

### استدعاءات الـ GraphQL للبرامج التفاعلية

#### 1. إضافة برنامج تفاعلي جديد:

```graphql
mutation createInteractiveProgram($name: String!, $attachments: Upload, $youtubeLink: String, $booksIds: [ID!], $seriesId: ID, $description: String, $keywords: String, $coverImage: Upload, $isActive: String!) {
  createInteractiveProgram(data: {
    name: $name,
    attachments: $attachments,
    youtubeLink: $youtubeLink,
    books: { connect: $booksIds },
    series: { connect: { id: $seriesId }},
    description: $description,
    keywords: $keywords,
    coverImage: $coverImage,
    isActive: $isActive
  }) {
    id
    name
    isActive
  }
}
```

#### 2. استعلام جلب البرامج التفاعلية:

```graphql
query getInteractivePrograms {
  interactivePrograms {
    id
    name
    youtubeLink
    books {
      title
    }
    series {
      name
    }
    description
    isActive
  }
}
```

#### 3. تحديث برنامج تفاعلي:

```graphql
mutation updateInteractiveProgram($id: ID!, $name: String, $attachments: Upload, $youtubeLink: String, $booksIds: [ID!], $seriesId: ID, $description: String, $keywords: String, $coverImage: Upload, $isActive: String) {
  updateInteractiveProgram(where: { id: $id }, data: {
    name: $name,
    attachments: $attachments,
    youtubeLink: $youtubeLink,
    books: { connect: $booksIds },
    series: { connect: { id: $seriesId }},
    description: $description,
    keywords: $keywords,
    coverImage: $coverImage,
    isActive: $isActive
  }) {
    id
    name
    isActive
  }
}
```

#### 4. حذف برنامج تفاعلي:

```graphql
mutation deleteInteractiveProgram($id: ID!) {
  deleteInteractiveProgram(where: { id: $id }) {
    id
    name
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع البرامج التفاعلية. إليك مثال لجلب قائمة البرامج التفاعلية:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: InteractiveProgramList(),
      ),
    );
  }
}

class InteractiveProgramList extends StatelessWidget {
  final String getInteractiveProgramsQuery = """
    query getInteractivePrograms {
      interactivePrograms {
        id
        name
        series {
          name
        }
        isActive
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Interactive Programs')),
      body: Query(
        options: QueryOptions(document: gql(getInteractiveProgramsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List interactivePrograms = result.data!['interactivePrograms'];

          return ListView.builder(
            itemCount: interactivePrograms.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(interactivePrograms[index]['name']),
                subtitle: Text('Series: ${interactivePrograms[index]['series']['name']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **المرفقات**: يمكن رفع ملفات متعددة مثل PDF أو صور.
- **رابط يوتيوب**: يسمح بإضافة رابط لفيديو يوتيوب لمزيد من الشروحات.
- **الكتب والسلاسل**: الربط مع المكتبة وسلاسل الدروس لتوفير محتوى متكامل.
- **الكلمات المفتاحية**: تساعد في تحسين البحث عن البرامج.

---

.

### **قسم الدروس وسلاسل الدروس (Lessons & Lesson Series)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

##### الدروس (Lesson):

| الحقل      | الوصف                                                                                          |
| --------------- | --------------------------------------------------------------------------------------------------- |
| `title`       | عنوان الدرس (نص مطلوب).                                                            |
| `description` | وصف مختصر للدرس.                                                                       |
| `videoLink`   | رابط الفيديو الخاص بالدرس (مطلوب).                                       |
| `coverImage`  | صورة غلاف الدرس.                                                                       |
| `publisher`   | الناشر المرتبط بالدرس (علاقة مع قائمة الناشرين).             |
| `relatedBook` | الكتاب المرتبط بالدرس (اختياري).                                          |
| `series`      | السلسلة التي ينتمي إليها الدرس (علاقة مع سلسلة الدروس). |
| `attachments` | المرفقات المرتبطة بالدرس (مثل ملفات PDF أو صور).                 |

##### سلاسل الدروس (Lesson Series):

| الحقل    | الوصف                                                                                  |
| ------------- | ------------------------------------------------------------------------------------------- |
| `name`      | اسم السلسلة (نص مطلوب).                                                    |
| `publisher` | الناشر المرتبط بالسلسلة (علاقة مع قائمة الناشرين). |
| `book`      | الكتاب المرتبط بالسلسلة (اختياري).                              |
| `isActive`  | حالة السلسلة (نشط أو غير نشط).                                        |

---

### استدعاءات الـ GraphQL للدروس وسلاسل الدروس

#### 1. إضافة درس جديد:

```graphql
mutation createLesson($title: String!, $description: String, $videoLink: String!, $publisherId: ID!, $relatedBookId: ID, $seriesId: ID, $attachmentsIds: [ID!]) {
  createLesson(data: {
    title: $title,
    description: $description,
    videoLink: $videoLink,
    publisher: { connect: { id: $publisherId }},
    relatedBook: { connect: { id: $relatedBookId }},
    series: { connect: { id: $seriesId }},
    attachments: { connect: $attachmentsIds }
  }) {
    id
    title
    videoLink
  }
}
```

#### 2. استعلام جلب الدروس:

```graphql
query getLessons {
  lessons {
    id
    title
    description
    videoLink
    publisher {
      name
    }
    series {
      name
    }
    attachments {
      file {
        url
      }
    }
  }
}
```

#### 3. إضافة سلسلة دروس جديدة:

```graphql
mutation createLessonSeries($name: String!, $publisherId: ID!, $bookId: ID, $isActive: String!) {
  createLessonSeries(data: {
    name: $name,
    publisher: { connect: { id: $publisherId }},
    book: { connect: { id: $bookId }},
    isActive: $isActive
  }) {
    id
    name
    isActive
  }
}
```

#### 4. استعلام جلب سلاسل الدروس:

```graphql
query getLessonSerieses {
  lessonSerieses {
    id
    name
    publisher {
      name
    }
    book {
      title
    }
    isActive
  }
}
```

#### 5. تحديث درس أو سلسلة دروس:

```graphql
mutation updateLesson($id: ID!, $title: String!, $videoLink: String!, $description: String, $publisherId: ID!, $relatedBookId: ID, $seriesId: ID, $attachmentsIds: [ID!]) {
  updateLesson(where: { id: $id }, data: {
    title: $title,
    videoLink: $videoLink,
    description: $description,
    publisher: { connect: { id: $publisherId }},
    relatedBook: { connect: { id: $relatedBookId }},
    series: { connect: { id: $seriesId }},
    attachments: { connect: $attachmentsIds }
  }) {
    id
    title
  }
}

mutation updateLessonSeries($id: ID!, $name: String!, $publisherId: ID!, $bookId: ID, $isActive: String!) {
  updateLessonSeries(where: { id: $id }, data: {
    name: $name,
    publisher: { connect: { id: $publisherId }},
    book: { connect: { id: $bookId }},
    isActive: $isActive
  }) {
    id
    name
  }
}
```

#### 6. حذف درس أو سلسلة دروس:

```graphql
mutation deleteLesson($id: ID!) {
  deleteLesson(where: { id: $id }) {
    id
    title
  }
}

mutation deleteLessonSeries($id: ID!) {
  deleteLessonSeries(where: { id: $id }) {
    id
    name
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع الدروس وسلاسل الدروس. إليك مثال لجلب قائمة الدروس:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: LessonList(),
      ),
    );
  }
}

class LessonList extends StatelessWidget {
  final String getLessonsQuery = """
    query getLessons {
      lessons {
        id
        title
        videoLink
        publisher {
          name
        }
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Lessons')),
      body: Query(
        options: QueryOptions(document: gql(getLessonsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List lessons = result.data!['lessons'];

          return ListView.builder(
            itemCount: lessons.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(lessons[index]['title']),
                subtitle: Text('Publisher: ${lessons[index]['publisher']['name']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **رابط الفيديو**: يُستخدم لعرض الفيديو المرفق بالدرس.
- **المرفقات**: يمكن ربط الدرس بملفات مثل PDF أو صور.
- **حالة السلسلة**: يمكن تحديد ما إذا كانت السلسلة نشطة أو غير نشطة.

---

.

### **قسم المكتبة (Library)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل     | الوصف                                                                                |
| -------------- | ----------------------------------------------------------------------------------------- |
| `title`      | اسم الكتاب (نص مطلوب).                                                    |
| `author`     | اسم الكاتب (نص مطلوب).                                                    |
| `publisher`  | الناشر المرتبط بالكتاب (علاقة مع قائمة الناشرين). |
| `bookFile`   | ملف الكتاب بصيغة PDF أو غيرها (يتم رفعه).                     |
| `coverImage` | صورة غلاف الكتاب (يتم رفعها).                                       |

---

### استدعاءات الـ GraphQL للكتب في المكتبة

#### 1. إضافة كتاب جديد:

```graphql
mutation createLibraryBook($title: String!, $author: String!, $publisherId: ID!, $bookFile: Upload!, $coverImage: Upload!) {
  createLibrary(data: {
    title: $title,
    author: $author,
    publisher: { connect: { id: $publisherId }},
    bookFile: $bookFile,
    coverImage: $coverImage
  }) {
    id
    title
    author
  }
}
```

#### 2. استعلام جلب الكتب:

```graphql
query getLibraryBooks {
  libraries {
    id
    title
    author
    publisher {
      name
    }
    bookFile {
      url
    }
    coverImage {
      url
    }
  }
}
```

#### 3. تحديث كتاب:

```graphql
mutation updateLibraryBook($id: ID!, $title: String!, $author: String!, $publisherId: ID!, $bookFile: Upload, $coverImage: Upload) {
  updateLibrary(where: { id: $id }, data: {
    title: $title,
    author: $author,
    publisher: { connect: { id: $publisherId }},
    bookFile: $bookFile,
    coverImage: $coverImage
  }) {
    id
    title
  }
}
```

#### 4. حذف كتاب:

```graphql
mutation deleteLibraryBook($id: ID!) {
  deleteLibrary(where: { id: $id }) {
    id
    title
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع الكتب في المكتبة. إليك مثال لجلب قائمة الكتب:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: LibraryBookList(),
      ),
    );
  }
}

class LibraryBookList extends StatelessWidget {
  final String getLibraryBooksQuery = """
    query getLibraryBooks {
      libraries {
        id
        title
        author
        publisher {
          name
        }
        bookFile {
          url
        }
        coverImage {
          url
        }
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Library Books')),
      body: Query(
        options: QueryOptions(document: gql(getLibraryBooksQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List books = result.data!['libraries'];

          return ListView.builder(
            itemCount: books.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(books[index]['title']),
                subtitle: Text('Author: ${books[index]['author']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **ملف الكتاب**: يتم تحميل الكتاب بصيغة PDF أو ملفات أخرى.
- **صورة الغلاف**: يمكن إضافة صورة غلاف مميزة للكتاب.
- **البحث عن الناشر**: يتم اختيار الناشر من قائمة الناشرين المرتبطة بالمشروع.

---

.

### **قسم التخزينات الإضافية (More Detail)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل    | الوصف                                                                                                             |
| ------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `name`      | اسم التعريف للبيانات المخزنة (نص مطلوب، يتم فهرسته لسهولة البحث). |
| `data`      | الحقل الذي سيخزن بيانات بصيغة JSON ولكن على شكل نص.                               |
| `createdAt` | وقت وتاريخ إنشاء السجل (يتم تعيينه تلقائيًا عند إنشاء السجل).         |

---

### استدعاءات الـ GraphQL للتخزينات الإضافية

#### 1. إضافة بيانات جديدة:

```graphql
mutation createMoreDetail($name: String!, $data: String!) {
  createMoreDetail(data: {
    name: $name,
    data: $data
  }) {
    id
    name
    createdAt
  }
}
```

#### 2. استعلام جلب البيانات المخزنة:

```graphql
query getMoreDetails {
  moreDetails {
    id
    name
    data
    createdAt
  }
}
```

#### 3. تحديث بيانات موجودة:

```graphql
mutation updateMoreDetail($id: ID!, $name: String!, $data: String!) {
  updateMoreDetail(where: { id: $id }, data: {
    name: $name,
    data: $data
  }) {
    id
    name
    createdAt
  }
}
```

#### 4. حذف بيانات:

```graphql
mutation deleteMoreDetail($id: ID!) {
  deleteMoreDetail(where: { id: $id }) {
    id
    name
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع التخزينات الإضافية. إليك مثال لجلب البيانات المخزنة:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: MoreDetailList(),
      ),
    );
  }
}

class MoreDetailList extends StatelessWidget {
  final String getMoreDetailsQuery = """
    query getMoreDetails {
      moreDetails {
        id
        name
        data
        createdAt
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('More Details')),
      body: Query(
        options: QueryOptions(document: gql(getMoreDetailsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List moreDetails = result.data!['moreDetails'];

          return ListView.builder(
            itemCount: moreDetails.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(moreDetails[index]['name']),
                subtitle: Text('Created at: ${moreDetails[index]['createdAt']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **تخزين بيانات JSON كنص**: هذا القسم يمكن أن يكون مفيدًا لتخزين بيانات مخصصة في شكل JSON، بحيث يمكن استرجاعها عند الحاجة.
- **سهولة البحث باستخدام الفهرسة**: الحقل `name` يتم فهرسته ليسهل البحث في السجلات المخزنة.

---

.

### **قسم الصفحات (Pages)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل         | الوصف                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------- |
| `title`          | عنوان الصفحة (نص مطلوب).                                                              |
| `content`        | نص الصفحة، يمكن أن يحتوي على HTML (مطلوب).                                  |
| `lastModifiedAt` | تاريخ آخر تعديل للصفحة (يتم تحديثه تلقائيًا عند التعديل). |

---

### استدعاءات الـ GraphQL لإدارة الصفحات

#### 1. إضافة صفحة جديدة:

```graphql
mutation createPage($title: String!, $content: String!) {
  createPage(data: {
    title: $title,
    content: $content
  }) {
    id
    title
    lastModifiedAt
  }
}
```

#### 2. استعلام جلب الصفحات:

```graphql
query getPages {
  pages {
    id
    title
    content
    lastModifiedAt
  }
}
```

#### 3. تحديث صفحة:

```graphql
mutation updatePage($id: ID!, $title: String!, $content: String!) {
  updatePage(where: { id: $id }, data: {
    title: $title,
    content: $content
  }) {
    id
    title
    lastModifiedAt
  }
}
```

#### 4. حذف صفحة:

```graphql
mutation deletePage($id: ID!) {
  deletePage(where: { id: $id }) {
    id
    title
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع الصفحات، مثل صفحة سياسة الخصوصية وغيرها. إليك مثال لجلب قائمة الصفحات:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: PageList(),
      ),
    );
  }
}

class PageList extends StatelessWidget {
  final String getPagesQuery = """
    query getPages {
      pages {
        id
        title
        content
        lastModifiedAt
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Pages')),
      body: Query(
        options: QueryOptions(document: gql(getPagesQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List pages = result.data!['pages'];

          return ListView.builder(
            itemCount: pages.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(pages[index]['title']),
                subtitle: Text('Last Modified: ${pages[index]['lastModifiedAt']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **نص الصفحة (HTML)**: يتم تخزين محتوى الصفحة على شكل HTML لعرضه لاحقًا مثل سياسة الخصوصية أو صفحة الشروط والأحكام.
- **تاريخ آخر تعديل**: يتم تحديثه تلقائيًا عند إجراء أي تعديل على الصفحة.

---

.

### **قسم المحادثات (Chat AI)**

---

#### قائمة المعلومات المتاحة للإرسال أو الاستلام عبر الـ API:

| الحقل      | الوصف                                                                                          |
| --------------- | --------------------------------------------------------------------------------------------------- |
| `chatName`    | اسم الدردشة (نص مطلوب).                                                            |
| `chatHistory` | سجل الدردشة (JSON).                                                                       |
| `user`        | المستخدم المرتبط بالدردشة (علاقة مع قائمة المستخدمين). |
| `createdAt`   | تاريخ إنشاء الدردشة (يتم تعيينه تلقائيًا عند الإنشاء).  |

---

### استدعاءات الـ GraphQL لإدارة المحادثات

#### 1. إضافة محادثة جديدة:

```graphql
mutation createChat($chatName: String!, $userId: ID!) {
  createChatAi(data: {
    chatName: $chatName,
    user: { connect: { id: $userId }},
    chatHistory: []
  }) {
    id
    chatName
    createdAt
  }
}
```

#### 2. استعلام جلب المحادثات:

```graphql
query getChats {
  chatAis {
    id
    chatName
    chatHistory
    user {
      name
    }
    createdAt
  }
}
```

#### 3. تحديث محادثة:

```graphql
mutation updateChat($id: ID!, $chatName: String!, $chatHistory: JSON!) {
  updateChatAi(where: { id: $id }, data: {
    chatName: $chatName,
    chatHistory: $chatHistory
  }) {
    id
    chatName
    chatHistory
  }
}
```

#### 4. حذف محادثة:

```graphql
mutation deleteChat($id: ID!) {
  deleteChatAi(where: { id: $id }) {
    id
    chatName
  }
}
```

---

### **التكامل مع Flutter**

للتكامل مع تطبيق Flutter، يمكنك استخدام مكتبة **graphql_flutter** للتفاعل مع المحادثات، مثل إنشاء دردشات جديدة أو استرجاع سجل الدردشة. إليك مثال لجلب المحادثات:

```dart
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() async {
  final HttpLink httpLink = HttpLink('https://makesbooks.com/api/graphql');

  final ValueNotifier<GraphQLClient> client = ValueNotifier(
    GraphQLClient(
      link: httpLink,
      cache: GraphQLCache(store: InMemoryStore()),
    ),
  );

  runApp(MyApp(client: client));
}

class MyApp extends StatelessWidget {
  final ValueNotifier<GraphQLClient> client;

  MyApp({required this.client});

  @override
  Widget build(BuildContext context) {
    return GraphQLProvider(
      client: client,
      child: MaterialApp(
        home: ChatList(),
      ),
    );
  }
}

class ChatList extends StatelessWidget {
  final String getChatsQuery = """
    query getChats {
      chatAis {
        id
        chatName
        createdAt
        user {
          name
        }
      }
    }
  """;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Chats')),
      body: Query(
        options: QueryOptions(document: gql(getChatsQuery)),
        builder: (QueryResult result, {refetch, fetchMore}) {
          if (result.hasException) {
            return Text(result.exception.toString());
          }

          if (result.isLoading) {
            return Center(child: CircularProgressIndicator());
          }

          List chats = result.data!['chatAis'];

          return ListView.builder(
            itemCount: chats.length,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(chats[index]['chatName']),
                subtitle: Text('User: ${chats[index]['user']['name']}'),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

### **بقية الأدوات:**

- **سجل الدردشة**: يتم تخزين سجل الدردشة في حقل JSON ليتم تحديثه أو استرجاعه لاحقًا.
- **ربط المستخدم**: كل محادثة مرتبطة بمستخدم واحد من خلال علاقة مع قائمة المستخدمين.

---

.
