1. **Instalar mysql:**
   `npm i mysql2`
   https://www.npmjs.com/package/mysql2

2. **Instalar sequelize:**
   `npm i sequelize`
   https://www.npmjs.com/package/sequelize

3. **Requerir sequelize**
   `const { Sequelize } = require('sequelize')`
   https://sequelize.org/docs/v6/getting-started/

4. **Instanciar sequelize**

```
const sequelize = new Sequelize('DATABASENAME', 'DATABASEUSER', 'PASSWORD', {
	host: 'localhost',
	dialect: 'mysql',
	port: 3306,
	logging: false,
})
```

5. **Comprobamos la conexión a la DataBase**

```
const checkConnection = async () => {
	try {
		await sequelize.authenticate()
		console.log('Conexión a la orden 🫡')
	} catch (error) {
		throw new Error(error)
	}
}
```

6. **Creamos un modelo (tabla) en carpeta _models > fichero.js_**

```
const { DataTypes } = require('sequelize');
const sequelize = require('***aqui va la ruta donde esté instanciado sequelize***')

const Receta = sequelize.define('receta', {
  nombre: {
    type: DataTypes.STRING
  },
    dificultad: {
    type: DataTypes.STRING
  },
    creadora: {
    type: DataTypes.STRING
  },
})

module.exports = Receta
```

**tip**: el nombre del modelo o tabla en singular 🧍‍♀️!!

**tip**: no añadas id, se crea solo 😉!!

7. **Sincronizar lo que he hecho con la base de datos en carpeta _models > fichero.js_**

```
const syncModels = async () => {
	try {
    await sequelize.sync({ force: true }); // {alter: true}
    console.log("Ya lo tienes todo en tu base de datos 🔄");
  } catch (error) {
		throw new Error(error)
	}
}
```

8. **Hacer un CRUD**
   https://sequelize.org/docs/v6/core-concepts/model-querying-basics/

8.1.**Crear 1**

```
const createOne = async () => {
	try{
		const receta = await Receta.create({
			nombre: 'Pollo al curry con arroz basmati',
			dificultad: 'Fácil',
			creadora: 'Diana Tabraue'
		})
		console.log(`'${receta.title}' insertada! 🥳`)
	}catch(error){
		throw new Error('Ooops algo no fue bien 😰')
	}
}
```

8.2. **Crear bulk**

```
const bulkCreate = async () => {
	try{
		const recetas = await Receta.bulkCreate([
            {
                nombre: 'Pollo al curry con arroz basmati',
                dificultad: 'Fácil',
                creadora: 'Diana Tabraue'
		    },
            {
                nombre: 'Pollo en salsa de ciruelas',
                dificultad: 'Medio',
                creadora: 'Diana Tabraue'
		    },
        ])
		console.log(`recetas insertadas! 🥳`)
	}catch(error){
		throw new Error('Ooops algo no fue bien 😰')
	}
}
```

8.3. **Update**

```
const updateRecipe = async => () {

	await Receta.update({dificultad: 'Fácil'},
        {
		where: {
			id: 2
		}
	});

	console.log(`Recipe updated 👌!`)
}
```

8.4. **Delete**

```
const deleteRecipe = async () => {

	await Receta.destroy({
		where: {
			id: 2
		}
	})

	console.log(`La borraste.... 😢`)
}
```

9. **Cerrar conexión**

```
const closeConnection = async () => {
	try {
		await sequelize.close()
		console.log('⛔ Bye Bye!! -- Database has been closed')
	}catch(error){
		throw new Error(error)
	}
}
```
