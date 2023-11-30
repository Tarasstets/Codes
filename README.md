// Массив(Матриця)




int main() {
	const int rows = 3;// ряд
 
	const int cols = 3;// стовпчик

	int arr[rows][cols]; // створюєм массив
	for (int i = 0; i < rows; i++) { // цикл на ряд
		for (int j = 0; j < cols; j++) { // цикл на стовпчик
			arr[i][j] = rand() % 20; // рандомні числа від 0 до 20
		}
	}
	for (int i = 0; i < rows; i++) {// це ми копіюєм 
		for (int j = 0; j < cols; j++) {// це тоже
			cout << arr[i][j] << "\t";// виводить числа массива через пробіл
		}
		cout << endl;// виводить числа массива з нового рядка
	}
	
	
}
